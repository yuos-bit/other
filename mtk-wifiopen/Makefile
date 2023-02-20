##############################################
# 
# OpenWrt Makefile for MTK-WIFI Automatic Open
#
##############################################
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME := MTK-WIFI-Open
PKG_VERSION := 1.0
PKG_RELEASE := 1
PKG_LICENSE:=GPLv3
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

# package的基本信息，根目录、子目录等
define Package/$(PKG_NAME)
  SECTION:=luci
  CATEGORY:=LuCI
  SUBMENU:=7. mtk-wifi-open
  PKGARCH:=all
  TITLE:=MTK-WIFI Automatic Open
  MAINTAINER:=Lemonade
endef

define Package/$(PKG_NAME)/description
	Lemonade's MTK-WIFI Automatic Open
endef

define Build/Prepare
endef

define Build/Configure
endef

define Build/Compile
endef

# 定义所有的安装路径、文件及其目标路径
define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/usr/share/wifiopen
	$(INSTALL_BIN) ./files/wifiopen.sh $(1)/etc/init.d/wifiopen
endef

$(eval $(call BuildPackage,mtkiappd))