# MiniVisor Project

## Overview
M-Visor is a virtualization framework that embeds a (Ring-2 layer) inside Windows.  
It uses CPU-native virtualization (Intel VMX / AMD SVM) to implement concealment and pop-up handling at a hardware-assisted level.

---

## Current Status
- **Intel platforms:** Fully supported and stable.  
- **AMD platforms:** Partial support; SVM handling and encryption are in a Testing Phase (90% Sucess Rate).  
- **Pop-up handling:** Implemented and functional.  
- **Requirement:** VMX/VTD (Intel) or SVM (AMD) must be enabled in BIOS/UEFI.  
- **Build environment:** EDK2 (UEFI Development Kit 2).

---

## Technical Summary
M-Visor sits conceptually between (Ring -2) and (Ring 0) by adding a (Ring -2) System Management Module (SMM) layer. It uses hardware virtualization to intercept selected kernel and device interactions while minimizing visibility from userland and standard kernel-level monitoring.

---

## Project Source Directory
- **M-VisorDXE** — Main program for Intel (VMX).  
- **M-VisorSvmDXE** — Main program for AMD (SVM).  
- **Hypervisor** — Auxiliary virtualization enhancement modules.  
- **HardwareCollector** — Hardware fingerprint collection utility.

---

## VTD / IOMMU Bypass
Currently as of right now we have a bypass for FaceIT & VGK **IOMMU/VTD Restrictions** both **Hardware** based and **Software** Based.
Would also work on other platforms like **ACE, CN-VGK, EAC...**

**Intent and behavior.**
- M-Visor interacts with device and DMA-related behaviors at the virtualization layer to support pop-up handling and concealment features.
- The project is designed to operate in systems where CPU virtualization features (VMX/SVM) & SMM (System Management Module) are available.
- On some platforms the project may observe device-mapped memory behavior that is affected by IOMMU configuration. Results vary between Intel and AMD platforms.

**Limitations.**
- MiniVisor does **not** include or endorse methods to disable or bypass VT-d / IOMMU protections.
- AMD support is explicitly partial. Some DMA and encryption interactions are not fully handled by the SVM implementation in this codebase.

**Risks and detection.**
- Interacting with DMA and low-level device mappings can trigger security/forensics alerts on systems with intact IOMMU protections and endpoint monitoring.
- Attempting to defeat IOMMU/VT-d is a security risk and may expose the system to data corruption or device malfunction.
- Running the project on production systems or systems you do not own may violate law, policy, or warranty.

**Safe testing guidance.**
- Test only in controlled lab environments. Use dedicated hardware or isolated virtual lab machines.  
- For basic compatibility testing, toggle VT-d / IOMMU and CPU virtualization flags in firmware to observe behavior.
- Use hardware and firmware logs and established OS tools to monitor DMA and device mappings while testing.

---

## Build Instructions
1. Install and configure the EDK2 build environment.  
2. Clone the MiniVisor repository into your workspace.  
3. Enable CPU virtualization in firmware (VMX or SVM).  
4. Build using the EDK2 toolchain.  
5. Deploy the compiled DXE modules according to your testing workflow.

---

## Security and Ethics
This project is intended for research and controlled experimentation. Do not deploy on systems without explicit authorization. Do not use the project to circumvent or disable hardware security features.
The authors may provide guidance for evasion of hardware protections.

---

## Author / Contact
M-V Development Team
Discord: **@sqltable**
(The project/source is for sale)
