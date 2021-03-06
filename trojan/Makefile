#
# Copyright (C) 2018-2020 wongsyrone
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
include $(TOPDIR)/.config

PKG_NAME:=trojan
PKG_VERSION:=1.16.0
PKG_RELEASE:=0

ifneq (,$(findstring 18.06,$(CONFIG_VERSION_REPO)))
PKG_RELEASE:=$(PKG_RELEASE)-18.06
OPENSSL_EMBEDED_1_1:=1
else
OPENSSL_EMBEDED_1_1:=0
endif

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/trojan-gfw/trojan.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=8606b7110fe79f8ab02d60c897f87ffb0a9b23f0
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
CMAKE_INSTALL:=1
PKG_BUILD_PARALLEL:=0

ifeq ($(OPENSSL_EMBEDED_1_1),1)
PKG_BUILD_DEPENDS:=openssl1.1
endif

PKG_LICENSE:=GPL-3.0

PKG_MAINTAINER:=NewClear

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

TARGET_CXXFLAGS += -Wall -Wextra
TARGET_CXXFLAGS += $(FPIC)

# LTO
TARGET_CXXFLAGS += -flto
TARGET_LDFLAGS += -flto

# CXX standard
TARGET_CXXFLAGS += -std=c++11

TARGET_CXXFLAGS := $(filter-out -O%,$(TARGET_CXXFLAGS)) -O3

TARGET_CXXFLAGS += -ffunction-sections -fdata-sections
TARGET_LDFLAGS += -Wl,--gc-sections

ifeq ($(OPENSSL_EMBEDED_1_1),1)
MY_OPENSSL_DIR:=$(BUILD_DIR)/openssl1.1_staging_dir/usr
CMAKE_FIND_ROOT_PATH := $(MY_OPENSSL_DIR);$(CMAKE_FIND_ROOT_PATH)
TARGET_CXXFLAGS := -I$(MY_OPENSSL_DIR)/include $(TARGET_CXXFLAGS)
TARGET_LDFLAGS := -L$(MY_OPENSSL_DIR)/lib $(TARGET_LDFLAGS)
endif


CMAKE_OPTIONS += \
	-DENABLE_MYSQL=OFF \
	-DENABLE_NAT=ON \
	-DENABLE_REUSE_PORT=ON \
	-DENABLE_SSL_KEYLOG=ON \
	-DENABLE_TLS13_CIPHERSUITES=ON \
	-DFORCE_TCP_FASTOPEN=OFF \
	-DSYSTEMD_SERVICE=OFF \
	-DBoost_DEBUG=ON \
	-DBoost_NO_BOOST_CMAKE=ON

ifeq ($(OPENSSL_EMBEDED_1_1),1)
CMAKE_OPTIONS += \
	-DOPENSSL_USE_STATIC_LIBS=TRUE
endif


define Package/trojan
	SECTION:=net
	CATEGORY:=Network
	TITLE:=An unidentifiable mechanism that helps you bypass GFW
	URL:=https://github.com/trojan-gfw/trojan
	DEPENDS:=+libpthread +libstdcpp \
		+boost-system +boost-program_options +boost-date_time
ifeq ($(OPENSSL_EMBEDED_1_1),0)
	DEPENDS += +libopenssl
endif

endef



define Package/trojan/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/trojan $(1)/usr/sbin/trojan
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/trojan.config $(1)/etc/config/trojan
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/trojan.init $(1)/etc/init.d/trojan
endef


$(eval $(call BuildPackage,trojan))
