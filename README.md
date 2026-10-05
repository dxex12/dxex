# DXEX — DirectX Extended

[![License: Proprietary / Closed Source](https://img.shields.io/badge/License-Proprietary%20%2F%20Closed%20Source-red.svg)](LICENSE)
[![Status: Active Development](https://img.shields.io/badge/Status-Active%20Development-brightgreen.svg)](https://github.com)
[![Platform: Windows 10 / 11](https://img.shields.io/badge/Platform-Windows%2010%20%2F%2011-blue.svg)](https://github.com)
[![Target API: Direct3D 12](https://img.shields.io/badge/Target%20API-Direct3D%2012-orange.svg)](https://github.com)
[![Patreon](https://img.shields.io/badge/Patreon-Support%20on%20Patreon-FF424D.svg?logo=patreon&logoColor=white)](https://www.patreon.com/RohitDev/posts/introducing-dxex-171326940?utm_medium=clipboard_copy&utm_source=copyLink&utm_campaign=postshare_creator&utm_content=join_link)

<p align="center">
  <img src="dx9&dx11-to-dx12.png" alt="DX9 & DX11 to DX12" width="600" />
</p>

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/rohitdev)

**DXEX** (standing for **Direct X Extended**) is a high-performance graphics translation layer suite designed to translate legacy and modern Microsoft DirectX graphics APIs directly to **native Direct3D 12 (D3D12)** on Windows. Developed and maintained by **Rohit Kumar**.

---
<p align="center">
   <a href="https://discord.gg/TyyDD3C7wQ"><img src="https://files.kick.com/images/channel-links/194199/image/conversion/eb3c378e-8c8a-4d42-9396-a2d12b8832fd-image.webp" alt="Join on discord" width="300" height /></a>
   <a href="https://www.patreon.com/RohitDev"><img src="https://img1.wsimg.com/isteam/ip/e2d51209-09c9-44ec-b3b9-60940e888431/Patreon%20logo%202023.png" alt="Join on Patreon" width="300" height /></a>
</p>


### 💖 Support the Project on Patreon
Support ongoing development, game compatibility testing, and bug fixes:  
👉 **[Join our Patreon & Read the Debut v0.3.0 Post](https://www.patreon.com/RohitDev/posts/introducing-dxex-171326940?utm_medium=clipboard_copy&utm_source=copyLink&utm_campaign=postshare_creator&utm_content=join_link)**

---

> [!IMPORTANT]
> **Repository Purpose & Licensing Notice**:  
> This repository is the **official public issue tracker, compatibility database, and release distribution portal** for the DXEX project.  
> **DXEX is proprietary software and remains closed source.** Source code is not hosted in this repository, and pull requests containing code changes will not be accepted.

---

## 🎯 Scope & Capabilities

DXEX bridges classic and modern PC titles to the Direct3D 12 runtime with ultra-low CPU overhead, modern memory management, and stutter-free shader caching:

```
+---------------------------------------------------------------------------------+
|                       Game Application (x86 / x64)                             |
|               (e.g., Skyrim LE, New Vegas, Fallout 4, GTA V)                    |
+---------------------------------------------------------------------------------+
                         |                               |
                         v                               v
            +-------------------------+     +-------------------------+
            |  DXEX9 (D3D9 → D3D12)   |     |  DXEX11 (D3D11 → D3D12) |
            |  Direct3D 9 Translation |     | Direct3D 11 Translation |
            +-------------------------+     +-------------------------+
                         \                               /
                          v                             v
+---------------------------------------------------------------------------------+
|                         DXEX Direct3D 12 Core Engine                            |
|  - Native DXBC/DXIL pipeline state compilation (zero Vulkan/SPIR-V overhead)    |
|  - D3D12MA (D3D12 Memory Allocator) high-efficiency VRAM suballocation         |
|  - Persistent disk pipeline state & shader cache (.dxexcache)                   |
|  - Native DXGI Presentation (IDXGISwapChain3/4)                                 |
+---------------------------------------------------------------------------------+
                                      |
                                      v
+---------------------------------------------------------------------------------+
|                     Direct3D 12 Hardware Driver & GPU                           |
|                         (NVIDIA / AMD / Intel)                                  |
+---------------------------------------------------------------------------------+
```

### 1. DXEX9 (Direct3D 9 to Direct3D 12)
* Direct translation of D3D9/D3D9Ex draw submission, state management, and textures.
* On-the-fly D3D9 shader bytecode (DXSO) to Direct3D 12 DXBC container translation.
* Fixes common D3D9 limitations: modern VRAM addressing, alpha-test specialization, depth bias correction, and elimination of 32-bit RAM address space crashes.

### 2. DXEX11 (Direct3D 11 to Direct3D 12)
* High-throughput translation of Direct3D 11 rendering contexts to D3D12 command lists.
* Multi-threaded command recording, resource barrier management, and modern descriptor heap pooling.
* Unlocks D3D12 features such as enhanced frame pacing, variable refresh rate, and hardware-accelerated presentation for DX11 titles.

---

## 🗺️ Roadmap & Planned Features

* **DXEX11 Core**: Direct3D 11 to Direct3D 12 translation engine preview.
* **Enhanced State Deduplication**: Extended PSO state grouping for reduced memory overhead in heavily modded titles.
* **DXVK-like Configuration Options**: Extended runtime configuration profiles for legacy mod frameworks (ENB, ReShade, SKSE).

---

## 🐛 Submitting Issues & Compatibility Reports

Use the [GitHub Issues tab](../../issues) to report bugs, visual artifacts, crashes, or performance regressions.

### Issue Categories
* **Crash Reports**: Game crashes on launch, during scene transitions, or during specific draw calls.
* **Graphical Glitches**: Flickering textures, z-fighting on floor decals/mats, transparency sorting issues, or missing shadows/lighting.
* **Compatibility Reports**: Let us know whether a game runs (`Playable`, `Ingame`, `Menu`, or `Crash`).
* **Feature Requests & Suggestions**: Configuration options, frame limiters, or vendor spoofing controls.

### What to Include in a Report
To help us diagnose and fix issues quickly, please provide:
1. **Game Title & Edition**: (e.g., *The Elder Scrolls V: Skyrim (Legendary Edition) v1.9.32.0.8*)
2. **Translation Layer Used**: `DXEX9` (D3D9 → D3D12) or `DXEX11` (D3D11 → D3D12)
3. **Hardware & OS Specifications**:
   * **GPU**: (e.g., NVIDIA GeForce RTX 3080 / AMD Radeon RX 6800 XT / Intel Arc A770)
   * **Driver Version**: (e.g., NVIDIA 555.85)
   * **Operating System**: (e.g., Windows 10 22H2 / Windows 11 23H2 64-bit)
4. **Log Files**: Attach `dxex.log` and the game-specific log (e.g., `TESV_d3d9.log`).
5. **Screenshots or Clips**: Visual comparisons showing the artifact.

---

## 🚀 Key Architectural Advantages

* **No Intermediate SPIR-V / No Vulkan Overhead**: Unlike translation wrappers that compile to SPIR-V and depend on Vulkan runtime drivers, DXEX targets Microsoft Direct3D 12 natively.
* **D3D12 Memory Allocator (D3D12MA)**: Suballocates memory pools to eliminate driver hitching and VRAM thrashing.
* **Asynchronous Pipeline Disk Cache (`.dxexcache`)**: Saves converted pipelines to disk to guarantee smooth, stutter-free frame delivery across repeat sessions.
* **Native DXGI SwapChain Integration**: Employs borderless and exclusive fullscreen presentation directly through system `IDXGISwapChain3` / `IDXGISwapChain4` interfaces.

---

## ⚖️ Legal Disclaimer & Trademarks

* **Non-Affiliation**: DXEX is an independent, third-party software project and is **not** affiliated, associated, authorized, endorsed by, or in any way officially connected with **Microsoft Corporation**, or any of its subsidiaries or affiliates. The official Microsoft website can be found at [https://www.microsoft.com](https://www.microsoft.com).
* **Trademarks**: "DirectX", "Direct3D", "D3D", "Windows", and their respective logos and marks are registered trademarks of Microsoft Corporation in the United States and/or other countries.
* **Nominative Fair Use**: All product names, logos, trademarks, game titles, and registered trademarks mentioned or referenced within this repository are the property of their respective owners. Any reference to these marks or names is done strictly for **identification, technical description, compatibility reporting, and nominative fair use** purposes. DXEX makes no claim of ownership or partnership with respect to any third-party intellectual property.

---

## 👨‍💻 Author & Maintainer

* **Author**: Developed and maintained by **Rohit Kumar**.
* **Project**: DXEX (DirectX Extended).
* **Discord**: [RohitDev on Discord](https://discord.gg/TyyDD3C7wQ)
* **Patreon**: [RohitDev on Patreon](https://www.patreon.com/RohitDev)
* **LinkTree**: [RohitDev on LinkTree](https://linktr.ee/rohitdev1)
* **Feedback & Support**: Please use the [GitHub Issues](../../issues) tab for bug reports, logs, and compatibility testing feedback.

---

## 🔍 SEO & Discovery Keywords

`DXEX` `DirectX Extended` `DXEX12` `Direct3D 9 to Direct3D 12` `Direct3D 11 to Direct3D 12` `D3D9 to D3D12` `D3D11 to D3D12` `DX9 to DX12` `DX11 to DX12` `DirectX 9 to DirectX 12 Translation Layer` `DirectX 11 to DirectX 12 Translation Layer` `D3D9On12` `D3D11On12` `DX9On12` `DX11On12` `Direct3D 12 Translation Layer` `DirectX 12 Wrapper` `DirectX 12 Compatibility Layer` `Direct3D 12 Proxy DLL` `d3d9.dll replacement` `d3d11.dll replacement` `D3D12 Memory Allocator` `D3D12MA` `Skyrim LE D3D12` `DirectX Bytecode Converter` `DXSO to DXBC` `GPU Graphics API Translation` `Windows 10 Gaming` `Windows 11 Gaming` `High Performance Graphics Translation` `DirectX Extended Issue Tracker`
