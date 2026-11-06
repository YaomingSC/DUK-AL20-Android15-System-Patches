# DUK-AL20 Android 15 GSI：设备补丁与已知问题

## 项目说明
本项目由 AI 协助完成，包括设备补丁编写、构建问题分析和镜像整理；设备所有者提供设备环境，执行刷写并进行实机功能验证。

本仓库只包含 DUK-AL20 / Hi3660 的 LineageOS 22.2 / Android 15 TrebleDroid GSI 设备补丁，不包含完整的 LineageOS/TrebleDroid 源码树、system 镜像、GApps、专有固件、日志或分区备份。可刷入的 system 镜像发布在 GitHub Releases。

## 包含的改动

- `device/generic/common/duk_al20_audio_node.rc`：将 Hi3660 的 NXP/TFA smart-PA 设备节点授权给 media 组。设备所有者确认声音正常。
- `device/generic/common/duk_al20_afbc_fix.rc`：在 early-init 阶段设置 `vendor.debug.gralloc_afbc=0`。设备所有者确认该设置后画面正常。
- `patches/gsi_arm64.mk.patch`：将上述两个 rc 文件加入系统镜像，并设置 `ro.build.shutdown_timeout=45`。关机超时属性已包含在测试构建中；普通重启测试也使用了另一仓库中的 KSU test4 内核，因此不能把重启表现单独归因于该属性。

## 构建背景

测试基线为 LineageOS 22.2 / Android 15 TrebleDroid GSI，`lineage_gsi_arm64`、`userdebug`。GApps 来自设备所有者提供的 MindTheGapps 包。

从上游源码开始，将本仓库中的两个 rc 文件放到 `device/generic/common/`，并在项目目录应用补丁：

```sh
git apply patches/gsi_arm64.mk.patch
```

这是源码补丁集，不是可直接刷入的完整 ROM。AFBC 设置只在 DUK-AL20 / Hi3660 上验证。

## 实机测试结果

设备所有者确认：系统可启动，声音正常；设置 `vendor.debug.gralloc_afbc=0` 后画面正常；镜像包含 GApps。搭配另一仓库中的 KSU test4 内核时，普通重启和进入 Fastboot 均成功，但切换/启动耗时较长。上述结果只适用于本次 DUK-AL20 实机测试。

## 已知问题

根据设备所有者实测，以下功能目前异常或未正常工作，尚未修复：

- NFC
- 指纹识别
- 蓝牙
- 基带/蜂窝通信
- 状态栏不显示实时网络流量

因此该系统并非功能完整的日用 ROM。请把它视作设备专用测试镜像；刷写前核对型号和分区布局，并准备好恢复方案。
