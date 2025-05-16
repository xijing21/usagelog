# 沁恒微电子MCU

## 现状

从硬件设备支持的情况来说：

| 支持方            | 分类               | 概况                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |  |  |
| ----------------- | ------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | - | - |
| 沁恒微电子        | Windows            | MounRiver Studio（基于VSCode开发）集成了编辑器、安装时提示安装烧录工具（WCH-Link）等工具；                                                                                                                                                                                                                                                                                                                                                                                                     |  |  |
| 沁恒微电子        | linux              | IDE：MounRiver Studio Linux版本<br />编译器（linux）：官网提供了二进制下载 [MRS_Toolchain_Linux_x64_V210.tar.xz](http://file-oss.mounriver.com/tools/MRS_Toolchain_Linux_x64_V210.tar.xz)  <br />                                    包含了openOCD、RISC-V Embedded GCC(8)、RISC-V Embedded GCC12<br />烧录工具（linux版本）：[wlink](https://github.com/ch32-rs/wlink) （https://github.com/ch32-rs/wlink）<br />程序：<br />① ide中输入设备型号，直接创建工程<br /> ② EVT包 |  |  |
| 沁恒微电子        | MacOS              | 情况类似linux                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  |  |  |
|                   |                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |  |  |
| PlatformIO        |                    | 参考：https://matrix.ruyisdk.org/zh_CN/board/CH32V103/FreeRTOS-README/                                                                                                                                                                                                                                                                                                                                                                                                                         |  |  |
|                   | 工具链             | case①沁恒微官网提供的工具链：MRS_Toolchain_Linux_x64_V210.tar.xz<br />case②开源的支持WCH开发板的工具链：https://github.com/xpack-dev-tools/gcc-xpack                                                                                                                                                                                                                                                                                                                                         |  |  |
|                   | 构建镜像           | 需要自己准备Makefile 或构建脚本[Makefile](https://matrix.ruyisdk.org/zh_CN/board/CH32V103/FreeRTOS-README/Makefile)                                                                                                                                                                                                                                                                                                                                                                               |  |  |
|                   | 烧录工具           | case①WCH-ISP：https://github.com/ch32-rs/wchisp<br />case②OpenOCD：                                                                                                                                                                                                                                                                                                                                                                                                                          |  |  |
|                   |                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |  |  |
| RuyiSDK下怎么弄？ |                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |  |  |
|                   | 工具链             | ①：最好是发布支持 WCH的 ruyisdk toolchain<br />②：二次分发沁恒官网提供的工具链<br />③：使用开源的支持WCH开发板的工具链（②和③可能有关联，除了技术问题，更多的是协作关系、协议许可等非技术问题）                                                                                                                                                                                                                                                                                            |  |  |
|                   | 构建镜像           | FreeRTOS RT-Thread ?                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |  |  |
|                   | 烧录               | ①分发wlink？<br />②OpenOCD？                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |  |  |
|                   | 应用程序（跑马灯） | 开发一个适用于WCH开发板（可以是特定某个型号）的程序                                                                                                                                                                                                                                                                                                                                                                                                                                            |  |  |
|                   |                    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |  |  |

https://github.com/community-pio-ch32v/platform-ch32v/tree/develop/examples/blinky-none-os

https://github.com/community-pio-ch32v/platform-ch32v/blob/develop/builder/main.py

## Linux下使用WCH

### 准备

- MounRiverStudio_Linux_X64_V210    点击下载:  .deb (x64)  |  .tar.xz (x64)  ：http://file-oss.mounriver.com/upgrade/MounRiverStudio_Linux_X64_V210.deb
- MRS_Toolchain_Linux_x64_V210.tar.xz : http://file-oss.mounriver.com/tools/MRS_Toolchain_Linux_x64_V210.tar.xz
- 
- wlink：https://github.com/ch32-rs/wlink.git
- 

### 安装

```
1. 安装mounriver studio ide
sudo dpkg -i MounRiverStudio_Linux_X64_V210.deb

2. 安装toolchain
# 方式一：安装ide后，自动就包含了，位置在 /usr/share/MRS2/MRS-linux-x64/resources/app/resources/linux/components/WCH/Toolchain 下
/usr/share/MRS2/MRS-linux-x64/resources/app/resources/linux/components/WCH
├── manifest.json
├── OpenOCD
├── Others
├── SDK
└── Toolchain

# 方式二：下载自安装
/home/phebe/wch/MRS_Toolchain_Linux_x64_V210
├── OpenOCD
├── RISC-V Embedded GCC
└── RISC-V Embedded GCC12

3. 安装wlink
#https://github.com/ch32-rs/wlink.git

sudo apt update
sudo apt install git pkg-config libudev-dev 
cargo install --git https://github.com/ch32-rs/wlink

echo $PATH | grep "$HOME/.cargo/bin"
echo 'export PATH="$HOME/.cargo/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
wlink --help



```

## RuyiSDK集成/支持方案

## 使用RuyiSDK在WCH上快捷开发
