# open core resource

oc 资源收藏整理

## 目的

构建适合 AMD 平台的 oc 配置

## 平台配置

硬件|型号
--- | ---
CPU| AMD Ryzen 3800x
GPU| AMD 5700XT
MotherBoard| Asus X570 TUF Gaming plus
Network| built-in
wifi| -
audio| built-in

## 项目结构

- resource 原始资源目录
- final 最终构建

## 项目版本

工具|版本
---|---
OC|0.5.6
apple support pkg|2.1.6
amd kext patch|release day 3.18
kext VirtualSMC.kext|1.1.1
kext Lilu.kext|1.4.2
kext WhateverGreen|1.3.7
kext AppleALC|1.4.7
kext RealtekRTL8111|2.2.2

## 部署准备

### BIOS

Disable:
- Fast Boot
- VT-d(can be enabled if you set DisableIoMapper to YES)
- CSM
- Intel SGX
- Intel Platform Trust

Enable:
- VT-x
- Above 4G decoding
- Hyper-Threading
- Execute Disable Bit
- EHCI/XHCI Hand-off
- OS type: Windows 8.1/10 UEFI Mode

### USB install

[link](https://khronokernel-2.gitbook.io/opencore-vanilla-desktop-guide/opencore-efi/winblows-install)

### EFI setup

- ACPI
    - SSDT-EC.aml （Windows下操作生成）

- Drivers
    必须的3者
    - FwRuntimeServices.efi （给boot.efi打补丁用）
    - ApfsDriverLoader.efi （APFS支持）
    - VBoxHfs.efi （mac OS安装和恢复用的hfs支持）

- Kexts
    必须的2者
    - lilu （很多打补丁都需要的内核）
    - VirtualSMC.kext （虚拟SMC支持）
    
    - WhateverGreen.kext （GPU补丁）
    - AppleALC.kext （音频补丁）
    - RealtekRTL8111.kext （有线网络）


## 部署过程


## Reference

- opencore 官方文档：[opencore-vanilla-desktop-guide](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/)
- AMD内核补丁配置：[AMD-Vanilla-Kernal-Patch](https://github.com/AMD-OSX/AMD_Vanilla/tree/opencore/17h)
- 关于ACPI：[Getting-Started-With-ACPI](https://khronokernel.github.io/Getting-Started-With-ACPI/)