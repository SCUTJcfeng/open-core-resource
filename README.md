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

最新参考 plist Booter Introduction P14
AMD-BIOS-Settings
Disable:
- Fast Boot
- Compatibility Support Module (CSM)(Must be off, GPU errors like gIO are common when this option in enabled)

Enable:
- Above 4G decoding(This must be on, if you can't find the option then add npci=0x2000 to boot-args. Do not have both this option and npci enabled at the same time)
EHCI/XHCI Hand-off
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
    - HfsPlus.efi （mac OS安装和恢复用的hfs支持）

- Kexts
    必须的2者
    - lilu （很多打补丁都需要的内核）
    - VirtualSMC.kext （虚拟SMC支持）
    
    - WhateverGreen.kext （GPU补丁）
    - AppleALC.kext （音频补丁）
    - RealtekRTL8111.kext （有线网络）

[see](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/AMD/AMD-config.html)
- config.plist
    - ACPI
        - ADD （fixme）
        - 其余默认，quirks全部为false
    - Booter
        - 全部默认 （quirks根据 AMD-config 一致）
    - DeviceProperties
        - 移除add和block的所有内容（iGPU和audio）
    - Kernal
        - ADD顺序在倒入property tree时已指定，这里检查一下就好
        - 移除block的列表
        - 先移除原来的Patch，再倒入0318-AMD-patch.plist里面的Patch
        - DummyPowerManagement, PanicNoKextDump, PowerTimeoutKernelPanic and XhciPortLimit 设为true，其余保持默认
    - Misc
        - Boot 默认
        - Debug 修改以获取更多debug消息
        - Security 修改 AllowNvramReset, AllowSetDefault, RequireSignature, RequireVault and ScanPolicy
        - Tools 不需要
        - Entries 待定
    - NVRAM
        - boot-args 添加 agdpmod=pikera （Navi GPU）
        - nvda_drv 删除
        - prev-lang:kbd: 7A682D48 616E733A 323532 （中文对应zh-Hans:252，不清楚为什么跟文档对不上）[link](https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt)
        其它默认
    - Platforminfo
        - GenSMBIOS type 1，3 then input iMacPro1,1 5 for generating 
        其它默认
    - UEFI
        - HFSPlus.efi
        - AudioDevice blank (fixme)
        - PointerSupportMode blank (ASUS)
        - RequestBootVarFallback Yes


## 部署过程


## Reference

- opencore 官方文档：[opencore-vanilla-desktop-guide](https://khronokernel.github.io/Opencore-Vanilla-Desktop-Guide/)
- AMD内核补丁配置：[AMD-Vanilla-Kernal-Patch](https://github.com/AMD-OSX/AMD_Vanilla/tree/opencore/17h)
- 关于ACPI：[Getting-Started-With-ACPI](https://khronokernel.github.io/Getting-Started-With-ACPI/)
- plist 全配置官方文档
