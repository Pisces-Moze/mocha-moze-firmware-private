# Mocha Moze 私有固件档案

这是 Pisces-Moze 的私人研究档案，**不是开源固件仓库**。不得把本仓库改为公开、镜像到公开仓库或将全部文件套用项目 GPL 许可。私有保存不改变厂商版权或许可，也不代表获得再分发授权。

## 来源与完整性

文件来自已下载并校验的官方 MIUI V9.2.4.0.KXFCNEK / Android 4.4，包名 `mocha_images_V9.2.4.0.KXFCNEK_20180406.0000.00_4.4_cn_0d819c67da.tgz`。官方 URL、包 SHA256、各文件原始路径、长度和 SHA256 在 [manifest.json](manifest.json)。二进制保持原样；未修改、未反编译、未重新授权。

## 目录

```text
miui-system/etc/firmware/           # BCM 蓝牙/Wi-Fi、TFA9890 DSP、BQ27520 电量计等
miui-system/etc/firmware/tegra12x/  # GK20A FECS/GPCCS、VIC、MSENC、TSEC 等原厂资产
miui-system/vendor/firmware/       # 官方包中的其他触控参考资产
manifest.json                     # 来源、许可状态、原始路径与文件哈希
SHA256SUMS                        # 二进制校验清单
NOTICES.md                        # 版权、许可与适配限制
tools/verify.py                   # 本地完整性核对，不安装任何文件
```

运行 `python3 tools/verify.py`，或在 Linux 中运行 `sha256sum -c SHA256SUMS`。参考文件不等于已适配：这个官方包也包含别的硬件/模式的资产，不能把所有文件都装到 Mocha Debian。

## 与公开工程配合

公开驱动、设备树和构建说明仍位于 [mocha-moze-debian](https://github.com/Pisces-Moze/mocha-moze-debian) 及其四个配套源码仓库。公开工程不引用本私有仓库作为必需构建依赖，不包含私有仓库下载 token。开发者只能按实际驱动日志请求的文件名、硬件 revision 与加载格式选择固件。

`tegra12x/fecs.bin`、`gpccs.bin` 等是旧 MIUI 打包格式，不等于 Nouveau 请求的 `nvidia/gk20a/fecs_inst.bin` 等拆分格式；不能简单改名替代。BCM4350C0.hcd 也必须核对实际蓝牙芯片 revision。BQ27520 的 ATL/LGC 更新脚本只存档，不自动运行；TFA .speaker/.preset/.eq 为官方 ROM 配套声学资产，不是本设备读出的 MTP 校准。

## 排除项

不收录 CUDA、L4T 用户态库、账号/SSH 密钥、设备读出的 MAC/NVRAM、分区备份和整包系统镜像。官方 `nvram.txt` 及 Wi-Fi 示例连接配置也排除，以免把板级身份/连接数据当成通用固件。这里只保存当前已提取且通过原始清单校验的非 CUDA 固件，不声称包含厂商未公开的源码或全部设备内部程序。
