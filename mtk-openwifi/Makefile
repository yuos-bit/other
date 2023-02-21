##############################################
# 
# OpenWrt Makefile for MTK-WIFI Automatic Open
#
##############################################
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=luci-app-openwifi
PKG_LICENSE:=GPLv3
PKG_MAINTAINER:=Xiao Yu <mail@yuos.top>

LUCI_TITLE:=LuCI support for mt wifi open
PKG_VERSION := 1.0
PKG_RELEASE := 1
PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=7.mtk wifiopen
	SUBMENU:=Wireless
	TITLE:=Lemonade's MTK-WIFI Automatic Open
	DEPENDS:=
	PKGARCH:=all
	URL:=https://www.ddnsto.com/
endef

define Package/$(PKG_NAME)/description
	Lemonade's MTK-WIFI Automatic Open
endef


# 定义所有的安装路径、文件及其目标路径
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/usr/share/wifiopen

	$(INSTALL_BIN) $(PKG_BUILD_DIR)/wifiopen $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/wifiopen.sh $(1)/etc/init.d/wifiopen
	sed -i 's/WIRELESS_IOCTL_PARAM/$(WIRELESS_IOCTL_PARAM)/' $(1)/etc/init.d/wifiopen
	$(INSTALL_DATA) ./files/firewall.include $(1)/usr/share/wifiopen/
endef

$(eval $(call BuildPackage,$(PKG_NAME))