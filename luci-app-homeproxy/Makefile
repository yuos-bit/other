# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2022-2023 ImmortalWrt.org

include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-homeproxy
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

LUCI_TITLE:=The modern ImmortalWrt proxy platform for ARM64/AMD64
LUCI_PKGARCH:=all

# 核心依赖：增加了 shadow-user 用于确保用户组存在
LUCI_DEPENDS:= \
	+sing-box \
	+firewall4 \
	+kmod-nft-tproxy \
	+ucode-mod-digest \
	+ucode-mod-uci \
	+ucode-mod-fs \
	+ucode-mod-ubus \
	+curl \
	+ca-bundle

define Package/luci-app-homeproxy/conffiles
/etc/config/homeproxy
/etc/homeproxy/certs/
/etc/homeproxy/ruleset/
/etc/homeproxy/resources/
/etc/homeproxy/scripts/
endef

# 安装后的脚本处理：解决 Permission Denied 的核心步骤
define Package/luci-app-homeproxy/postinst
#!/bin/sh
if [ -z "$${IPKG_INSTROOT}" ]; then
	# 1. 确保脚本具备执行权限
	chmod +x /etc/homeproxy/scripts/*.sh
	chmod +x /etc/homeproxy/scripts/*.uc
	
	# 2. 预创建运行目录并修正归属
	mkdir -p /var/run/homeproxy
	mkdir -p /etc/homeproxy/resources
	mkdir -p /etc/homeproxy/ruleset
	
	# 3. 确保 sing-box 用户存在 (由依赖包创建，此处做二次确认/赋权)
	chown -R sing-box:sing-box /etc/homeproxy
	chown -R sing-box:sing-box /var/run/homeproxy
	
	# 4. 修正日志文件权限
	touch /var/run/homeproxy/homeproxy.log
	chown sing-box:sing-box /var/run/homeproxy/homeproxy.log
fi
exit 0
endef

include $(TOPDIR)/feeds/luci/luci.mk

# call BuildPackage - OpenWrt buildroot signature