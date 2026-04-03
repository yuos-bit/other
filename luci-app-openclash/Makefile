include $(TOPDIR)/rules.mk

PKG_NAME:=luci-app-openclash
PKG_VERSION:=0.47.075
PKG_RELEASE:=135
PKG_MAINTAINER:=vernesong <https://github.com/vernesong/OpenClash>

# 内核下载配置
CORE_VER:=1.19.22
CORE_FILE:=mihomo-linux-amd64-v3-v$(CORE_VER).gz
CORE_URL:=https://github.com/MetaCubeX/mihomo/releases/download/v$(CORE_VER)/$(CORE_FILE)

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)/config
    config PACKAGE_kmod-inet-diag
    default y if PACKAGE_$(PKG_NAME)

    config PACKAGE_luci-compat
    default y if PACKAGE_$(PKG_NAME)

    config PACKAGE_kmod-nft-tproxy
    default y if PACKAGE_firewall4

    config PACKAGE_kmod-ipt-nat
    default y if ! PACKAGE_firewall4

    config PACKAGE_iptables-mod-tproxy
    default y if ! PACKAGE_firewall4

    config PACKAGE_iptables-mod-extra
    default y if ! PACKAGE_firewall4

    config PACKAGE_dnsmasq_full_ipset
    default y if ! PACKAGE_firewall4

    config PACKAGE_dnsmasq_full_nftset
    default y if PACKAGE_firewall4

    config PACKAGE_ipset
    default y if ! PACKAGE_firewall4
endef

define Package/$(PKG_NAME)
    CATEGORY:=LuCI
    SUBMENU:=3. Applications
    TITLE:=LuCI support for clash
    PKGARCH:=all
    DEPENDS:=+dnsmasq-full +bash +curl +ca-bundle +ip-full \
    +ruby +ruby-yaml +kmod-tun +unzip
    MAINTAINER:=vernesong
endef

define Package/$(PKG_NAME)/description
    A LuCI support for clash
endef

define Build/Prepare
    $(CP) $(CURDIR)/root $(PKG_BUILD_DIR)
    $(CP) $(CURDIR)/luasrc $(PKG_BUILD_DIR)
    $(foreach po,$(wildcard ${CURDIR}/po/zh-cn/*.po), \
        po2lmo $(po) $(PKG_BUILD_DIR)/$(patsubst %.po,%.lmo,$(notdir $(po)));)

    # 下载并处理 mihomo 内核
    mkdir -p $(PKG_BUILD_DIR)/downloads
    wget -nc -O $(PKG_BUILD_DIR)/downloads/$(CORE_FILE) $(CORE_URL)
    zcat $(PKG_BUILD_DIR)/downloads/$(CORE_FILE) > $(PKG_BUILD_DIR)/clash_meta
    chmod +x $(PKG_BUILD_DIR)/clash_meta

    chmod 0755 $(PKG_BUILD_DIR)/root/etc/init.d/openclash
    chmod -R 0755 $(PKG_BUILD_DIR)/root/usr/share/openclash/
    mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/config
    mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/rule_provider
    mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/proxy_provider
    mkdir -p $(PKG_BUILD_DIR)/root/etc/openclash/core
    mkdir -p $(PKG_BUILD_DIR)/root/usr/share/openclash/backup/overwrite
    cp -f "$(PKG_BUILD_DIR)/root/etc/config/openclash" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_rules.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_rules.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_rules_2.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_rules_2.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_hosts.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_hosts.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_fake_filter.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_fake_filter.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_domain_dns.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_domain_dns.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_domain_dns_policy.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_domain_dns_policy.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_proxy_server_dns_policy.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_proxy_server_dns_policy.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_fallback_filter.yaml" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_fallback_filter.yaml" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_sniffer.yaml" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_sniffer.yaml" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_localnetwork_ipv4.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_localnetwork_ipv4.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_localnetwork_ipv6.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_localnetwork_ipv6.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_chnroute_pass.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_chnroute_pass.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_chnroute6_pass.list" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_chnroute6_pass.list" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_firewall_rules.sh" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_firewall_rules.sh" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/custom/openclash_custom_overwrite.sh" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/openclash_custom_overwrite.sh" >/dev/null 2>&1
    cp -f "$(PKG_BUILD_DIR)/root/etc/openclash/overwrite/default" "$(PKG_BUILD_DIR)/root/usr/share/openclash/backup/overwrite/default" >/dev/null 2>&1
endef

define Build/Configure
endef

define Build/Compile
endef

# 声明持久化配置目录和关键文件，防止重启和升级丢失
define Package/$(PKG_NAME)/conffiles
/etc/config/openclash
/etc/openclash/core/
/etc/openclash/Country.mmdb
/etc/openclash/GeoSite.dat
/etc/openclash/GeoIP.dat
/etc/openclash/Model.bin
/etc/openclash/custom/
endef

define Package/$(PKG_NAME)/preinst
#!/bin/sh
    if [ -f "/etc/config/openclash" ] && [ ! -f "/tmp/openclash.bak" ]; then
        cp -f "/etc/config/openclash" "/tmp/openclash.bak" >/dev/null 2>&1
        cp -rf "/etc/openclash" "/tmp/openclash" >/dev/null 2>&1
        cp -rf "/usr/share/openclash/ui" "/tmp/openclash_ui" >/dev/null 2>&1
        cp -rf "/www/luci-static/resources/openclash/pac" "/tmp/pac" >/dev/null 2>&1
    fi
    exit 0
endef

define Package/$(PKG_NAME)/postinst
#!/bin/sh
    # 强制关闭小内存模式，防止启动脚本将内核搬运到 /tmp (RAM)
    if [ -f "/etc/config/openclash" ]; then
        uci -q set openclash.config.small_flash_memory='0'
        uci -q commit openclash
    fi

    # 清理残余的软链接（若存在且指向 /tmp 则删除，确保目录为物理目录）
    for target in /etc/openclash/core /etc/openclash/Country.mmdb /etc/openclash/GeoSite.dat /etc/openclash/GeoIP.dat; do
        [ -L "$$target" ] && rm -f "$$target"
    done
    
    mkdir -p /etc/openclash/core
    exit 0
endef

define Package/$(PKG_NAME)/prerm
#!/bin/sh
    [ -n "$(pidof clash)" ] && /etc/init.d/openclash stop 2>/dev/null
    if [ -f "/etc/config/openclash" ] && [ ! -f "/tmp/openclash.bak" ]; then
        cp -f "/etc/config/openclash" "/tmp/openclash.bak" >/dev/null 2>&1
        cp -rf "/etc/openclash" "/tmp/openclash" >/dev/null 2>&1
        cp -rf "/usr/share/openclash/ui" "/tmp/openclash_ui" >/dev/null 2>&1
        cp -rf "/www/luci-static/resources/openclash/pac" "/tmp/pac" >/dev/null 2>&1
    fi
    exit 0
endef

define Package/$(PKG_NAME)/postrm
#!/bin/sh
    DEFAULT_DNSMASQ_CFGID="$$(uci -q show "dhcp.@dnsmasq[0]" | awk 'NR==1 {split($0, conf, /[.=]/); print conf[2]}' 2>/dev/null)"
    if [ -f "/tmp/etc/dnsmasq.conf.$DEFAULT_DNSMASQ_CFGID" ]; then
       DNSMASQ_CONF_DIR="$$(awk -F '=' '/^conf-dir=/ {print $2}' "/tmp/etc/dnsmasq.conf.$DEFAULT_DNSMASQ_CFGID" 2>/dev/null)"
    else
       DNSMASQ_CONF_DIR="/tmp/dnsmasq.d"
    fi
    DNSMASQ_CONF_DIR=$${DNSMASQ_CONF_DIR%*/}
    rm -rf /etc/openclash >/dev/null 2>&1
    rm -rf /etc/config/openclash >/dev/null 2>&1
    rm -rf /tmp/openclash.log >/dev/null 2>&1
    rm -rf /tmp/openclash_start.log >/dev/null 2>&1
    rm -rf /tmp/openclash_last_version >/dev/null 2>&1
    rm -rf /tmp/openclash.change >/dev/null 2>&1
    rm -rf /tmp/clash_last_version >/dev/null 2>&1
    rm -rf /usr/share/openclash >/dev/null 2>&1
    rm -rf $${DNSMASQ_CONF_DIR}/dnsmasq_openclash_custom_domain.conf >/dev/null 2>&1
    rm -rf $${DNSMASQ_CONF_DIR}/dnsmasq_openclash_chnroute_pass.conf >/dev/null 2>&1
    rm -rf $${DNSMASQ_CONF_DIR}/dnsmasq_openclash_chnroute6_pass.conf >/dev/null 2>&1
    rm -rf /tmp/etc/openclash >/dev/null 2>&1
    rm -rf /tmp/openclash_announcement >/dev/null 2>&1
    rm -rf /www/luci-static/resources/openclash >/dev/null 2>&1
    sed -i '/OpenClash Append/,/OpenClash Append End/d' "/usr/lib/lua/luci/model/network.lua" >/dev/null 2>&1
    sed -i '/.*kB maximum content size*/c\HTTP_MAX_CONTENT      = 1024*100      -- 100 kB maximum content size' /usr/lib/lua/luci/http.lua >/dev/null 2>&1
    sed -i '/.*kB maximum content size*/c\export let HTTP_MAX_CONTENT = 1024*100;       // 100 kB maximum content size' /usr/share/ucode/luci/http.uc >/dev/null 2>&1
    uci -q delete firewall.openclash
    uci -q commit firewall
    [ -f "/etc/config/ucitrack" ] && {
    uci -q delete ucitrack.@openclash[-1]
    uci -q commit ucitrack
    }
    rm -rf /tmp/luci-*
    exit 0
endef

define Package/$(PKG_NAME)/install
    $(INSTALL_DIR) $(1)/usr/lib/lua/luci/i18n
    $(INSTALL_DATA) $(PKG_BUILD_DIR)/*.*.lmo $(1)/usr/lib/lua/luci/i18n/
    $(CP) $(PKG_BUILD_DIR)/root/* $(1)/
    $(CP) $(PKG_BUILD_DIR)/luasrc/* $(1)/usr/lib/lua/luci/
    
    # 安装 mihomo 内核到物理存储路径
    $(INSTALL_DIR) $(1)/etc/openclash/core
    $(INSTALL_BIN) $(PKG_BUILD_DIR)/clash_meta $(1)/etc/openclash/core/clash_meta
endef

$(eval $(call BuildPackage,$(PKG_NAME)))