# Changelog

All notable changes to **DXEX** (DirectX Extended) will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

---

## [Unreleased]

## [0.4.0] - 2026-10-03

### Added
- **Triple Buffering (`BufferCount = 3`)**:
  - Configured hardware DXGI swapchain (`DXGI_SWAP_EFFECT_FLIP_DISCARD`) with triple buffering (`kSwapBufferCount = 3`) in `Presenter::createOrResizeSwapchainLocked` and `wrapSwapchainImagesLocked`.
  - Smooths frame pacing, prevents presentation stalls against VBlank cycles, and matches high-performance rendering standards.

- **Dedicated Present Frame Thread (`dxex-frame`)**:
  - Enabled asynchronous presentation thread (`m_frameThread`) using Translation Layer's `WaitForMaximumFrameLatency` for frame latency pacing.

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
  - Added native Texture $\leftrightarrow$ Buffer copies via D3D12 placed footprints in the shared translation layer (`ResourceCopyRegion`).
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
- **Direct3D 9 to Direct3D 12 Translation Layer (`DXEX9`)**:
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
