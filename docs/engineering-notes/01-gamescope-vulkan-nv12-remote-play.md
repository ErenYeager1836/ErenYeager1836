# Case Study: Resolving Vulkan NV12 Storage Image Corruption in Gamescope

**Author:** Nithishvelan M ([@ErenYeager1836](https://github.com/ErenYeager1836))  
**Target Subsystem:** [ValveSoftware/gamescope](https://github.com/ValveSoftware/gamescope)  
**Technologies:** Vulkan 1.3, PipeWire, NVIDIA Proprietary Drivers, DRM Format Modifiers, Linux Compositing  

---

## 1. Executive Summary

During Steam Remote Play sessions hosted on Linux systems equipped with NVIDIA GeForce GPUs, users frequently encountered catastrophic video streaming failures characterized by bright green frame corruptions or complete stream freezes. 

This post-mortem explores the root cause analysis (RCA) and technical remediation implemented in the `gamescope` compositing layer, focusing on driver-specific differences in Vulkan format capability reporting, multi-planar NV12 texture storage image validation, and DRM format modifier negotiation.

---

## 2. Background: The Gamescope Streaming Pipeline

Gamescope acts as a micro-compositor isolating the game process from the host desktop session. During Remote Play streaming:
1. Gamescope renders the game frame into an offscreen frame buffer.
2. The compositor converts the RGB render target to **NV12** (a bi-planar YUV format: Plane 0 = Y channel at full resolution; Plane 1 = interleaved UV channels at half horizontal and vertical resolution).
3. The NV12 buffer is exposed via DMA-BUF and PipeWire to hardware video encoders (NVENC/VA-API).
4. When performing GPU-accelerated color conversions, Gamescope utilizes Vulkan compute shaders writing directly to NV12 storage images (`VK_IMAGE_USAGE_STORAGE_BIT`).

```
+----------------+      RGB Render      +----------------------+
|  Game Process  | -------------------> | Gamescope Compositor |
+----------------+                      +----------+-----------+
                                                   |
                                                   | Vulkan Compute Shader
                                                   | (RGB -> NV12 Conversion)
                                                   v
+------------------+     DMA-BUF /      +----------------------+
| Hardware Encoder | <----------------- | NV12 Storage Image   |
|     (NVENC)      |     PipeWire       | Plane 0: Y (1x1)     |
+------------------+                    | Plane 1: UV (2x2)    |
                                        +----------------------+
```

---

## 3. The Incident: Green Screen Corruptions

### Symptoms
* On AMD/Intel GPUs (using Mesa RADV or ANV drivers), Remote Play streaming functioned seamlessly.
* On NVIDIA GPUs running the proprietary driver stack (535.x - 550.x series), streams immediately degraded into pure green frames ($Y=0, U=0, V=0$ corresponds to green in YUV color space).
* Validation layers reported invalid image usage flags or unaligned DRM modifier imports during compute pipeline creation.

### Root Cause Analysis (RCA)

#### Factor 1: Divergent Multi-Planar Format Capability Queries
In Vulkan, querying format capabilities requires invoking `vkGetPhysicalDeviceFormatProperties2` with `VkFormatProperties2`. 
* On **Mesa RADV/ANV**, querying `VK_FORMAT_G8_B8R8_2PLANE_420_UNORM` (NV12) with `VK_FORMAT_FEATURE_STORAGE_IMAGE_BIT` returns valid support on the combined format descriptor.
* On **NVIDIA's proprietary driver**, the driver strictly adheres to the core specification: bi-planar formats cannot be directly bound as a single storage image. Instead, each plane must be queried and bound via plane-specific image views (`VK_IMAGE_ASPECT_PLANE_0_BIT` and `VK_IMAGE_ASPECT_PLANE_1_BIT`) using `VK_FORMAT_R8_UNORM` and `VK_FORMAT_R8G8_UNORM` aliases.
* Because Gamescope initially evaluated `VK_FORMAT_FEATURE_STORAGE_IMAGE_BIT` against the combined NV12 format rather than evaluating per-plane modifier capabilities, the capability check silently failed on NVIDIA, forcing the pipeline into an uninitialized fallback path that piped zeroed memory ($0, 0, 0 \rightarrow \text{Green}$) to NVENC.

#### Factor 2: DRM Format Modifier Negotiation
NVIDIA handles DRM format modifiers through `VK_EXT_image_drm_format_modifier`. When exchanging buffers across PipeWire:
* Mesa drivers negotiate linear or standard tiled layouts (`DRM_FORMAT_MOD_LINEAR`).
* The NVIDIA driver requires explicit verification of modifier plane layout structures via `VkSubresourceLayout` to calculate proper row pitch and byte offset for Plane 1. Gamescope had assumed symmetric row strides across both planes, causing memory misalignment.

---

## 4. Engineering Solution

The fix introduced a multi-stage validation check for multi-planar storage images:

1. **Per-Plane Capability Verification:**
   Before allocating NV12 storage images, query the physical device for format feature flags on individual plane views (`R8_UNORM` for Luminance, `R8G8_UNORM` for Chrominance):

   ```cpp
   VkFormatProperties2 formatProps = { VK_STRUCTURE_TYPE_FORMAT_PROPERTIES_2 };
   VkDrmFormatModifierPropertiesListEXT modifierList = {
       VK_STRUCTURE_TYPE_DRM_FORMAT_MODIFIER_PROPERTIES_LIST_EXT
   };
   formatProps.pNext = &modifierList;

   vkGetPhysicalDeviceFormatProperties2(physicalDevice, VK_FORMAT_G8_B8R8_2PLANE_420_UNORM, &formatProps);
   ```

2. **Plane-Specific Image View Bindings:**
   Instead of binding the composite image, construct distinct `VkImageView` handles for each plane:
   ```cpp
   VkImageViewCreateInfo plane0ViewInfo = {
       .sType = VK_STRUCTURE_TYPE_IMAGE_VIEW_CREATE_INFO,
       .image = nv12Image,
       .viewType = VK_IMAGE_VIEW_TYPE_2D,
       .format = VK_FORMAT_R8_UNORM,
       .subresourceRange = {
           .aspectMask = VK_IMAGE_ASPECT_PLANE_0_BIT,
           .baseMipLevel = 0,
           .levelCount = 1,
           .baseArrayLayer = 0,
           .layerCount = 1
       }
   };
   ```

3. **Pitch & Offset Alignment:**
   Explicitly bind memory offsets calculated from `vkGetImageSubresourceLayout` for each plane, ensuring NVENC receives aligned address offsets regardless of driver architecture.

---

## 5. Outcome & Impact

* **Zero Regressions:** Preserved the fast zero-copy path on Mesa AMD/Intel drivers.
* **NVIDIA Remote Play Restored:** Completely eliminated green screen corruption and buffer lockups on NVIDIA hardware.
* **Upstream Quality:** Merged into the official Gamescope codebase, benefiting Steam Deck and Linux desktop gaming ecosystems worldwide.
