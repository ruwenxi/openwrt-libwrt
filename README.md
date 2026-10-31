# 360 V6 / 京东云 AX6600 — LibWrt NSS 云编译

基于 [laipeng668/openwrt-ci-roc](https://github.com/laipeng668/openwrt-ci-roc) 精简，只保留一个编译流程。

## 说明

- 源码：`laipeng668/openwrt-6.x`，分支 `25.12-nss`，6.12 内核，两台设备统一使用 NSS 固件 12.5
- 默认管理地址：`10.1.1.1`，用户 `root`，默认密码为空，**首次登录后请立即设置密码**
- 设备名称（主机名）保持源码默认，不做修改

| 设备 | 内置插件 |
| --- | --- |
| 360 V6（`qihoo_360v6`） | OpenClash、DDNS-Go |
| 京东云 AX6600 雅典娜（`jdcloud_re-cs-02`） | OpenClash、DDNS-Go、OpenList2、Aria2（含 AriaNg）、athena-led |

两个设备共有的插件写在 `General.config`；AX6600 专属插件写在 `360v6-jd6600.config` 的 `DEVICE_PACKAGES` 里。

## 文件结构

| 路径 | 作用 |
| --- | --- |
| `.github/workflows/360v6-jd6600.yml` | 入口，在 Actions 手动运行 |
| `.github/workflows/Build-OpenWrt.yml` | 被入口调用的通用构建流程，不要删 |
| `configs/360v6-jd6600.config` | 设备与 NSS 配置 |
| `configs/General.config` | 通用配置与插件选择 |
| `scripts/Roc-script.sh` | 默认 IP、拉取第三方源码、编译前检查 |

## 增减插件

1. 两个设备都要的插件：在 `configs/General.config` 里写 `CONFIG_PACKAGE_xxx=y`；不要的写成 `=n`，仅加 `#` 无效。
   只给某个设备用的插件：在 `configs/360v6-jd6600.config` 里写 `CONFIG_PACKAGE_xxx=m`，并把包名加进该设备的 `DEVICE_PACKAGES`。
2. 如果插件不在 feeds 里，需要在 `scripts/Roc-script.sh` 里用 `package_enabled` 加一段 `clone_repository`。
3. 到 `Actions` 运行 `360v6-jd6600-LibWrt`，固件在 `Releases` 的 `360v6-jd6600` 标签下载。
