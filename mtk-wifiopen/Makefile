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
  SUBMENU:=MTK WIFI Open
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
	$(INSTALL_DIR) $(1)/etc/init.d/
	$(INSTALL_BIN) ./wifiopen $(1)/etc/init.d/
endef

define Package/$(PKG_NAME)/preinst
	#!/bin/bash
	echo 'installing $(PKG_NAME)'
endef

# 安装时执行命令创建软连接
define Package/$(PKG_NAME)/postinst
	#!/bin/sh
	# check if we are on real system
	if [ -z "$${IPKG_INSTROOT}" ]; then
		echo "Enabling rc.d symlink for $(PKG_NAME)"
		/etc/init.d/wifiopen enable
	fi

	echo '$(PKG_NAME) installed successed !'
	exit 0
endef


# 卸载时执行命令移除软连接
define Package/$(PKG_NAME)/prerm
	#!/bin/sh
	# check if we are on real system
	if [ -z "$${IPKG_INSTROOT}" ]; then
		echo "Removing rc.d symlink for $(PKG_NAME)"
		/etc/init.d/wifiopen disable
	fi
	echo 'removeing $(PKG_NAME)'
	exit 0
endef

define Package/$(PKG_NAME)/postrm
	#!/bin/bash
	echo '$(PKG_NAME) remove successed !'
endef

$(eval $(call BuildPackage,$(PKG_NAME)))