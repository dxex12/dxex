# Changelog

All notable changes to **DXEX** (DirectX Extended) will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).


## [0.9.0] - 2026-10-23

### Added
- **D3D9 Mods**: Added compatiblity for d3d9 mods
- **GamePlug D3D9**: Added compatiblity for GamePlug d3d9 layer
- **ENB**: Added compatiblity for ENB

---

## [0.8.0] - 2026-10-22

### Performance
- Increased FPS and smoother frame pacing.
- Reduced CPU overhead and thread synchronization latency.
- Reduced VRAM usage.
- Reduced RAM usage.

---

## [0.7.0] - 2026-10-21

### Added
- **Hardware Instancing**: Added hardware geometry instancing support with dedicated pipeline state cache keying.
- **Multi-Planar Subresources**: Added multi-planar format and subresource view routing across texture surfaces.
- **Verification Suite**: Automated runtime verification suite for dynamic multisample antialiasing and anisotropic filtering.
- **Texture 3D**: Added texture 3D support for games like Resident Evil 6.

### Performance
- Increased FPS and smoother frame pacing via faster PSO compilation.

### Fixed
- **Skyrim LE**: Fixed graphical glitches in outside world sky and floor.
- **MSAA Presentation**: Fixed multisample backbuffer presentation and resolve hazards during swapchain display.
- **Dynamic Render States**: Fixed dynamic antialiasing and line smoothing render state updates.
- **Input Assembler Aliasing**: Fixed vertex input assembler aliasing causing visual sky streaks and mesh distortions.
- **Anisotropic Filtering**: Fixed anisotropic sampler filter state detection causing blurry distant textures.
- **Sky Dome Clipping & Fog**: Fixed sky dome geometry clipping and driver-level fog emulation conflicts.
- **Shader Linkage**: Fixed shader input signature linkage matching and rasterizer state specialization.
- **Instanced Offsets**: Fixed geometry stream offset alignment during instanced draw calls.

### Improved
- **Depth Bias Behavior**: Aligned depth bias behavior with legacy Direct3D specifications.
- **Texture Sampling**: Enhanced texture sampling LOD clamping and descriptor stability.

---

## [0.6.0] - 2026-10-18

### Remarks
- **Nvidia Drivers**: Driver >= 616.56 recommended (Nvidia resolves 32-bit D3D12 driver crash [5725350]).
- **4GB (LAA) Patch**: Heavy 32-bit games must be 4GB-patched (default 2GB is too tight).

### Added
- **64-bit D3D9**: Added 64-bit (x64) Direct3D 9 support.

### Performance
- Increased FPS and smoother frame pacing via better resource handling and adaptive synchronization.
- Reduced VRAM churn and memory fragmentation across staging buffers.

### Fixed
- **Double D3D12 Device Creation**: Eliminated redundant device instantiation on startup, cutting VRAM overhead.
- **Alt-Tab Hang**: Fixed GPU hang and window freeze when Alt-Tabbing or switching focus.
- **Double Cursor**: Prevented Windows desktop cursor from showing over in-game cursor after Alt-Tab.

---

## [0.5.0] - 2026-10-12

### Added
- **Fallout: New Vegas Support**:
  - Added support for fallout new vegas
- **Fast Copy Region**:
  - Direct GPU copy path in `copyBufferToImage` for aligned buffer-to-texture uploads, bypassing CPU staging overhead.
- **MSAA Support**:
  - Native multisample view creation for RTV, DSV, and SRV (`sampleCount > 1`).
  - Bypassed 1-sample staging tile clears on multisampled resources.
  - Propagated sample count/quality to PSO keys and enabled Nvidia Alpha-To-Coverage (ATOC).
- **Buffer Upload Safety**:
  - Null checks for mapped vertex/index buffer slices in `UploadPerDrawData`.

### Fixed
- **`dxilconv.dll` Crash (`0xc0000005`)**:
  - Padded 2D VS texture coordinate outputs to 4D to match downstream 4D PS interpolator reads.
- **MSAA Capability Enumeration**:
  - Fixed `D3DMULTISAMPLE_NONMASKABLE` quality level queries for launcher/menu options.
  - Removed false warning on 16x MSAA modes on hardware capping multisampling at 8x.
- **Interior Cell Transition GPU Hang (`DXGI_ERROR_DEVICE_HUNG`)**:
  - Bounded swapchain frame queue depth (< 3) to prevent loading screen overrun.
  - Added self-copy guards in presentation blitter and copy commands.
  - Enforced matching subresource descriptors before whole-resource `ResourceCopy`.
  - Added object trimming on idle wait.

## [0.4.0] - 2026-10-05

### Added
- **Triple Buffering (`BufferCount = 3`)**:
  - Configured hardware DXGI swapchain (`DXGI_SWAP_EFFECT_FLIP_DISCARD`) with triple buffering (`kSwapBufferCount = 3`) in `Presenter::createOrResizeSwapchainLocked` and `wrapSwapchainImagesLocked`.
  - Smooths frame pacing, prevents presentation stalls against VBlank cycles, and matches high-performance rendering standards.

- **Dedicated Present Frame Thread (`dxex-frame`)**:
  - Enabled asynchronous presentation thread (`m_frameThread`) using  `WaitForMaximumFrameLatency` for frame latency pacing.

### Fixed
- **Descriptor Heap Exhaustion (`max=4096`)**:
  - Expanded CBV/SRV/UAV heap capacity from 4,096 to 65,536 and implemented free-list recycling across CBV/SRV/UAV, RTV, and DSV heaps.
- **Crash on Exit (`TESV.exe+0x15e05f`)**:
  - Resolved `0xc0000005` null dereference in `TESIdleForm` destructor on shutdown via startup patch and VEH recovery.

---

## [0.3.0] - 2026-10-03

### Fixed
- **Particle System Flickering (Smoke, Campfires, Sparks)**:
  - Implemented multi-slot ring buffering for dynamic vertex and index buffers (`D3DUSAGE_DYNAMIC`).
  - Advances buffer slice offset on `D3DLOCK_DISCARD` to prevent concurrent GPU/CPU draw overwrites.
  - Automatically re-binds active IA vertex/index buffer views upon discard invalidation.
- **Save Game Freeze & Command List Crash**:
  - Added native Texture $\leftrightarrow$ Buffer copies via D3D12 placed footprints in the shared Resource Copy Region.
  - Fixed D3D12 error `ID=854` (dimension limit) and command list fault during `GetRenderTargetData` thumbnail captures.
  - Added `D3D12_HEAP_TYPE_READBACK` support for staging buffers and enforced 512-byte subresource placement alignment.

---

## [0.2.0] - 2026-10-03

### Fixed
- **Visual Flickering on Foliage & Alpha Cards**:
  - Integrated native alpha testing (`discard_z`) into the D3D9-to-D3D12 shader converter (`ShaderConv`).
  - Completely eliminated visual flickering and transparency fighting on tree leaves, pine needles, grass blades, character hair cards, and wall moss.
- **Z-Fighting on Floor Mats, Rugs & Decals**:
  - Synchronized D3D9 `DepthBias` and `SlopeScaledDepthBias` directly into D3D12 `D3D12_RASTERIZER_DESC` within the Graphics Pipeline State Object (PSO).
  - Fixed z-fighting on floor mats, tavern carpets, blood splatters, and wall decals.
- **1-Frame Visual Flashing (Constant Buffer Wrap Desync)**:
  - Fixed ring-buffer offset desync by capturing the exact allocation offset directly from `D3D9ConstantBuffer::Alloc` upon buffer wrap.
  - Eliminated occasional single-frame transform spikes, corrupted terrain lighting, and character model distortions.
- **Startup Crash (`0xc0000409 STATUS_STACK_BUFFER_OVERRUN`)**:
  - Resolved `D3DHAL_SAMPLER_MAXSAMP` struct packing discrepancy in `RasterStates` across compilation units.
  - Decoupled `PSSamplers` from external D3DHAL macros to prevent stack canary corruption in `TranslateVertexShader`.
  - Added compile-time static assertions ensuring identical 36-byte `RasterStates` footprint across all translation units.

### Changed
- **Optimized Distribution Binary Size**:
  - Stripped internal debug symbols (`.pdb`) from release builds, reducing `d3d9.dll` binary size from ~3.5 MB to **~1.7 MB** for reduced memory footprint and faster load times.
- **Specialized Pipeline State Keying**:
  - Updated `LinkagePatchedPsKey` and `DxexGraphicsPipelineStateInfo` with `alphaTestEnable` and `alphaFunc` to prevent pipeline state pollution between alpha-tested and non-alpha-tested geometry.

---

## [0.1.0] - 2026-09-15

### Added
- **Direct3D 9 to Direct3D 12 Graphic Translation Api (`DXEX9`)**:
  - Full Direct3D 9 / Direct3D 9Ex API frontend wrapper (`d3d9.dll`).
  - Native DXSO (Direct3D 9 Shader Object) to DXBC (DirectX Byte Code) in-flight converter.
  - Zero SPIR-V / Vulkan runtime dependencies.
- **D3D12 Memory Allocator (`D3D12MA`)**:
  - Suballocated memory heaps for GPU textures, index buffers, and vertex buffers to eliminate runtime allocation stalls.
- **Asynchronous Shader & Pipeline Disk Cache (`.dxexcache`)**:
  - Persistent binary disk caching for compiled Direct3D 12 Graphics Pipeline States (PSOs), providing hitch-free repeat gameplay sessions.
- **Direct DXGI Presentation Integration**:
  - Direct integration with native Windows DXGI (`IDXGISwapChain3` / `IDXGISwapChain4`) supporting borderless and exclusive fullscreen display modes.
- **DXBC Shader Signing Integration**:
  - Automatic cryptographic hashing and signature generation via `dxbcSigner.dll` to ensure full D3D12 driver compatibility.
