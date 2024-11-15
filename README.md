# CloseWRT-CI
云编译CloseWRT固件

CloseWRT源码：
https://github.com/VIKINGYFY/CloseWRT-CI.git

HANWCKF源码：
https://github.com/hanwckf/immortalwrt-mt798x.git

PADAVANONLY源码：
https://github.com/padavanonly/immortalwrt-mt798x.git

AABANDON源码：
https://github.com/Aabandon/immortalwrt-mt798x.git

lgs2007m:
https://github.com/lgs2007m

天灵:
https://github.com/1715173329

# 固件简要说明：

固件每天早上4点自动编译。

固件信息里的时间为编译开始的时间，方便核对上游源码提交时间。

MEDIATEK系列，配套的UBOOT：
https://github.com/VIKINGYFY/UBOOT-CI/releases

# 目录简要说明：

workflows——自定义CI配置

Scripts——自定义脚本

Config——自定义配置

## 如何刷入
参考 https://t.me/nanopi_r2s/637 刷入单分区版 GPT BL2 FIP, 再通过 custom U-Boot 刷写 sysupgrade.bin 固件
> 已增加 CMCC RAX3000M eMMC 版 U-Boot，GPT BL2 FIP 刷入方式如下：
> ```
> dd if=mt7981-cmcc_rax3000m-emmc-gpt.bin of=/dev/mmcblk0 bs=512 seek=0 count=34 conv=fsync
> echo 0 > /sys/block/mmcblk0boot0/force_ro
> dd if=/dev/zero of=/dev/mmcblk0boot0 bs=512 count=8192 conv=fsync
> dd if=mt7981-cmcc_rax3000m-emmc-bl2.bin of=/dev/mmcblk0boot0 bs=512 conv=fsync
> dd if=/dev/zero of=/dev/mmcblk0 bs=512 seek=13312 count=8192 conv=fsync
> dd if=mt7981-cmcc_rax3000m-emmc-fip.bin of=/dev/mmcblk0 bs=512 seek=13312 conv=fsync
> ```
> 对应 ImmortalWrt CMCC RAX3000M eMMC version (custom U-Boot layout)、Q-WRT、及其他 eMMC 单分区版固件。

~~路由器进入 uboot 需要手动设置本机 IP 192.168.1.100 网关 192.168.1.1 DNS 192.168.1.1，~~ 新版 custom U-Boot 已支持 DHCP，浏览器输入 `192.168.1.1` 进入 Web-UI 刷写固件，所有文件可在 https://firmware.download.immortalwrt.eu.org/uboot/mediatek 获取

## 注意事项
此分区布局默认不创建 eMMC 闪存最后一块 56G 大分区，你需要使用 `cfdisk /dev/mmcblk0` 为最后一块剩余空闲容量手动创建 /dev/mmcblk0p7 分区并通过 mkfs.ext4 格式化以挂载使用，此后更新刷入其他固件则无需再进行相同操作，固件可以自动挂载
