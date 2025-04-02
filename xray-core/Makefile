include $(TOPDIR)/rules.mk

PKG_NAME:=xray-core
PKG_VERSION:=1.5.3
PKG_RELEASE:=1

# 无源码，不需要下载，跳过哈希校验
PKG_SOURCE:=
PKG_HASH:=skip

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Your Name <you@example.com>
PKG_LICENSE:=MPL-2.0
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/xray-core
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Xray Core
  DEPENDS:=+ca-bundle
endef

define Package/xray-core/description
  Xray, Penetrates Everything. It helps you to build your own computer network.
endef

# 编译阶段直接跳过
define Build/Compile
	@echo "跳过编译阶段，直接使用预编译的 xray 二进制文件。"
endef

define Package/xray-core/install
	# 复制预编译的 xray 到 /usr/bin
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(CURDIR)/xray $(1)/usr/bin/xray

	# 安装核心配置文件和初始化脚本
	$(INSTALL_DIR) $(1)/etc/xray
	$(INSTALL_DATA) $(CURDIR)/files/config.json.example $(1)/etc/xray/

	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_CONF) $(CURDIR)/files/xray.conf $(1)/etc/config/xray

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(CURDIR)/files/xray.init $(1)/etc/init.d/xray
endef

define Package/xray-example
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Xray Example Configs
  DEPENDS:=xray-core
  PKGARCH:=all
endef

define Package/xray-example/description
endef

define Package/xray-example/install
	# 安装示例配置文件到 /etc/xray
	$(INSTALL_DIR) $(1)/etc/xray/
	$(INSTALL_DATA) $(CURDIR)/files/vpoint_socks_vmess.json $(1)/etc/xray/
	$(INSTALL_DATA) $(CURDIR)/files/vpoint_vmess_freedom.json $(1)/etc/xray/
endef

$(eval $(call BuildPackage,xray-core))
$(eval $(call BuildPackage,xray-example))
