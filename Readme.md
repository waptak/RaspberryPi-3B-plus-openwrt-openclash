# 树莓派3B+刷`openwrt`和安装`openclash`说明

* ### 对应target:`brcm2708`, `bcm2710`
* ### 对应package: `aarch64_cortex-a53`

### 1、使用balenaEtcher写入镜像到TF卡并启动，提示修改`root`密码

### 2、安装中文包，开启`SFTP`
```bash
opkg update
# sftp服务
opkg install vsftpd openssh-sftp-server
# 中文包
opkg install luci-i18n-base-zh-cn
```
### 3、安装`open clash`,可直接看原项目安装步奏， [项目地址](https://github.com/vernesong/OpenClash)
```bash

#安装依赖
* luci
* luci-base
* iptables
* dnsmasq-full
* coreutils
* coreutils-nohup
* bash
* curl
* jsonfilter
* ca-certificates
* ipset
* ip-full
* iptables-mod-tproxy
* kmod-tun(TUN模式)
* luci-compat(Luci-19.07)

#上传IPK文件至您路由器的 /tmp 目录下

#假设安装包名字为
luci-app-openclash_0.43.01-beta_all.ipk

#执行安装命令
opkg install /tmp/luci-app-openclash_0.43.01-beta_all.ipk

#执行卸载命令
#插件在卸载后会自动备份配置文件到 /tmp 目录下，除非路由器重启，在下次安装时将还原您的配置文件
opkg remove luci-app-openclash

安装完成后刷新LUCI页面，在菜单栏 -> 服务 -> OpenClash 进入插件页面

```
### 4、安装好以后 `需要退出，再重新登录，才能看到服务菜单`

> ## 安装问题
* 如果安装报`dnsmasq-full` 错误，我是重启了一下，再装就好了，不保证所有都是这样处理，应该就是要用`dnsmasq-full`，到路由后台看下如果是`installed`,应该也就可以
  ```bash
  # 重启
  reboot

  opkg remove dnsmasq && opkg install dnsmasq-full

  opkg install /tmp/luci-app-openclash_0.43.01-beta_all.ipk
  ```

* 启动clash问题， `libcap` 版本不对， `libcap-bin`未安装，使用本项目里`libcap`的都上传到`tmp`目录重新安装一遍即可。也可以去[官方](https://downloads.openwrt.org/snapshots/packages/aarch64_cortex-a53/base/)或者[腾讯lede镜像](https://mirrors.cloud.tencent.com/lede/snapshots/packages/aarch64_cortex-a53/base/)下载
  ```bash
  # 安装
  opkg install /tmp/libcap-bin_2.51-1_aarch64_cortex-a53.ipk
  # 强制重装
  opkg install /tmp/libcap_2.51-1_aarch64_cortex-a53.ipk --force-depends --force-overwrite
  ```
