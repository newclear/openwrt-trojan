# openwrt-trojan

## Intro

[Trojan](https://github.com/trojan-gfw/trojan) package for OpenWrt.

Project is original forked from [trojan-gfw/openwrt-trojan](https://github.com/trojan-gfw/openwrt-trojan) project and following changes have been made:

- Move configs into /etc/config/trojan and generate config.json while launch.
- Supporting multiple instance of trojan process with different config.
- Add .travis.yml for automatically build and deploy for Travis.
- Checking OpenWrt SDK version:
  - using embed openssl 1.1 for OpenWrt 18.06
  - using system provided openssl for other version of OpenWrt (eg. 19.07)

## Download

Prebuild package can be download from [Release](https://github.com/newclear/openwrt-trojan/releases).

Prebuild package is automatically build and deploy dy Travis.

For every arch, there are two packages:

- `trojan-<version>-<arch>.ipk` for OpenWrt 19.07
- `trojan-<version>-18.06-<arch>.ipk` for OpenWrt 18.06

## Original README


```
openwrt-trojan
==============

Usage
---

1. copy these two folders to <openwrt-source-tree>/package.

2. install feeds from openwrt official package repository.

    ./scripts/feeds update -a
    ./scripts/feeds install -a

3. use 'make menuconfig' to select trojan package

4. the buildroot generate trojan binary linked to our openssl.
   You may use 'make package/trojan/{clean,compile} V=99' or
   whatever you like.

5. edit '/etc/config/trojan' file to enable it.
   The init script is disabled by default to avoid startup
   before configuration.

FAQ
---

Q: May I use openssl from openwrt?
A: As long as you don't need cutting-edge features, e.g. TLS 1.3.
   BTW, the Makefile doesn't depend on official openssl package.
```
