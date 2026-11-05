# DUK-AL20 Android 15 GSI：已验证的系统补丁

本仓库只包含设备所有者确认过的 LineageOS 22.2 / Android 15 GSI 系统改动：声音正常、画面不再闪烁。仓库不包含完整的 LineageOS/TrebleDroid 源码树、system 镜像、GApps、专有固件、日志或分区备份。

## 包含的改动

- `device/generic/common/duk_al20_audio_node.rc`：将 Hi3660 的 NXP/TFA smart-PA 设备节点授权给 media 组。设备所有者确认测试系统声音正常。
- `device/generic/common/duk_al20_afbc_fix.rc`：在 early-init 阶段设置 `vendor.debug.gralloc_afbc=0`。设备所有者确认此设置后画面正常。
- `patches/gsi_arm64.mk.patch`：将以上两个 rc 文件加入系统镜像，并设置 `ro.build.shutdown_timeout=45`。该设置包含在测试构建中；重启测试使用了另一个仓库中的 KSU test4 内核，不能据此断言这个属性单独修复了重启。

## 构建背景

测试基线为 LineageOS 22.2 / Android 15 TrebleDroid GSI，`lineage_gsi_arm64`、`userdebug`。GApps 来自设备所有者提供的 MindTheGapps 包，本仓库不包含该包。

从合法的上游源码开始，将本仓库中的两个 rc 文件放到 `device/generic/common/`，并在该项目目录应用补丁：

```sh
git apply patches/gsi_arm64.mk.patch
```

这是源码补丁集，不是可直接刷入的 ROM。AFBC 设置只在 DUK-AL20 / Hi3660 上验证过。

## 实机验证范围

设备所有者确认测试系统声音正常，画面正常且没有明显闪烁。搭配另一仓库中的 KSU test4 内核时，两次普通 Android 重启都回到了 Android，ADB 约 90 秒后重新连接。这表明两次重启完成，不代表重启很快或长期稳定。

该仓库只提供成功功能对应的设备补丁，不是完整 GSI 源码的逐字节复刻。NFC、性能及其他设备均不在已验证范围内。
