# All rights reserved.
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=mt7628e
P4REV:=8
PKG_VERSION:=4.0.5.0
PKG_BUILD_DIR:=$(KERNEL_BUILD_DIR)/mt7628

PKG_KCONFIG:= \
	RT_FIRST_CARD  RT_SECOND_IF_MT7628E RT_SECOND_CARD  AP_LLTD AP_LED AP_WDS AP_MBSS AP_MBSS_NEW_MBSSID_MODE  \
	AP_APCLI AP_80211AC_VHT AP_TXBF_SUPPORT  \

PKG_CONFIG_DEPENDS:=$(foreach c, $(PKG_KCONFIG),$(if $(CONFIG_$c),CONFIG_$(c)))

include $(INCLUDE_DIR)/package.mk

TAR_CMD=$(HOST_TAR) -C $(1)/ $(TAR_OPTIONS)

define KernelPackage/mt7628e
  CATEGORY:=Kernel modules
  TITLE:=MTK MT7628e wifi AP driver
  DEPENDS:=@TARGET_ramips
  FILES:=$(PKG_BUILD_DIR)/mt7628_ap/mt7628_ap.ko
  SUBMENU:=Wireless Drivers
  MENU:=1
endef

define KernelPackage/mt7628e/config
	source "$(SOURCE)/config.in"
endef

define Build/Compile
	$(MAKE) -C "$(LINUX_DIR)" V=1 \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		ARCH="$(LINUX_KARCH)" \
		M="$(PKG_BUILD_DIR)/mt7628_ap" \
		$(foreach c, $(PKG_KCONFIG),$(if $(CONFIG_MT7628E_$c),CONFIG_$(c)=$(CONFIG_MT7628E_$(c)))) \
		modules
endef

define KernelPackage/mt7628e/install
	$(INSTALL_DIR) $(1)/etc/wireless/mt7628/
endef

$(eval $(call KernelPackage,mt7628e))
