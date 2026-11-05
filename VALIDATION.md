# 实机验证记录

- 设备：HONOR V9 / DUK-AL20（Hi3660，berkeley）
- 系统：LineageOS 22.2 / Android 15 GSI（`lineage_gsi_arm64`，`userdebug`）
- 测试构建：r11 audio-node 系统构建；GApps 使用设备所有者提供的 MindTheGapps 包，本仓库不含该包。
- 设备所有者确认：声音正常；应用 AFBC 属性后画面正常、无明显闪烁。
- 重启测试使用另一仓库中的 KSU test4 内核：两次普通 Android 重启均回到 Android，ADB 约 90 秒后重新连接。
- 本仓库不是完整 GSI 源码复刻；不对 NFC、性能或其他设备作出修复声明。
