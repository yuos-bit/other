#
# Copyright (C) 2021 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=tailscale
PKG_VERSION:=1.84.2
PKG_RELEASE:=1

# 不再需要从源码包下载，直接使用本地文件
# PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
# PKG_SOURCE_URL:=file://./
# PKG_HASH:=skip

PKG_MAINTAINER:=Zephyr Lykos <self@mochaa.ws>, \
                Sandro Jäckel <sandro.jaeckel@gmail.com>

PKG_LICENSE:=BSD-3-Clause
PKG_LICENSE_FILES:=LICENSE
PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./tailscaled-linux-mipsle $(PKG_BUILD_DIR)/
endef

define Build/InstallDev
endef

define Build/Compile
endef

define Build/Configure
endef

define Package/tailscale
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=Zero config VPN
  URL:=https://tailscale.com
  DEPENDS:= +ca-bundle +kmod-tun
  PROVIDES:=tailscaled
endef

define Package/tailscale/description
  It creates a secure network between your servers, computers,
  and cloud instances. Even when separated by firewalls or subnets.
endef

define Package/tailscale/install
	$(INSTALL_DIR) $(1)/usr/sbin $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/tailscaled-linux-mipsle $(1)/usr/sbin/tailscaled
	$(LN) tailscaled $(1)/usr/sbin/tailscale
endef

$(eval $(call BuildPackage,tailscale))
