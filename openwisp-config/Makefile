# openwisp.org
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.

include $(TOPDIR)/rules.mk

PKG_NAME:=openwisp-config
PKG_VERSION:=$(shell cat ../VERSION)
PKG_RELEASE:=1
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(BUILD_VARIANT)/$(PKG_SOURCE_SUBDIR)
PKG_LICENSE:=GPL-3.0

include $(INCLUDE_DIR)/package.mk

define Package/openwisp-config/default
	TITLE:=Remote configuration management agent ($(2) variant)
	CATEGORY:=Administration
	SECTION:=admin
	SUBMENU:=openwisp
	DEPENDS:=+curl +lua +libuci-lua +luafilesystem $(3)
	VARIANT:=$(1)
	PKGARCH:=all
	MAINTAINER:=Federico Capoano <nemesis@ninux.org>
	URL:=http://openwisp.org
endef

Package/openwisp-config-openssl=$(call Package/openwisp-config/default,openssl,OpenSSL,+ca-certificates +libopenssl)
Package/openwisp-config-mbedtls=$(call Package/openwisp-config/default,mbedtls,mbedTLS,+ca-certificates +libmbedtls)
Package/openwisp-config-cyassl=$(call Package/openwisp-config/default,cyassl,CyaSSL,+ca-certificates +libcyassl)
Package/openwisp-config-nossl=$(call Package/openwisp-config/default,nossl,No SSL)

define Build/Compile
endef

define Package/openwisp-config-$(BUILD_VARIANT)/conffiles
    /etc/config/openwisp
endef

CONFIG_OPENWISP_UCI:=ssl

ifeq ($(BUILD_VARIANT),nossl)
    CONFIG_OPENWISP_UCI:=nossl
endif

define Package/openwisp-config-$(BUILD_VARIANT)/install
	$(INSTALL_DIR) \
		$(1)/usr/sbin \
		$(1)/etc/init.d \
		$(1)/etc/config \
		$(1)/etc/openwisp \
		$(1)/usr/lib/lua/openwisp

	$(INSTALL_BIN) \
		files/openwisp.agent \
		$(1)/usr/sbin/openwisp_config

	$(INSTALL_BIN) \
		files/openwisp.init \
		$(1)/etc/init.d/openwisp_config

	$(INSTALL_CONF) files/openwisp-$(CONFIG_OPENWISP_UCI).config \
		$(1)/etc/config/openwisp

	$(INSTALL_BIN) \
		files/sbin/openwisp-reload-config \
		$(1)/usr/sbin/openwisp-reload-config

	$(INSTALL_BIN) \
		files/lib/openwisp/utils.lua \
		$(1)/usr/lib/lua/openwisp/utils.lua

	$(INSTALL_BIN) \
		files/lib/openwisp/net.lua \
		$(1)/usr/lib/lua/openwisp/net.lua

	$(INSTALL_BIN) \
		files/sbin/openwisp-store-unmanaged.lua \
		$(1)/usr/sbin/openwisp-store-unmanaged

	$(INSTALL_BIN) \
		files/sbin/openwisp-restore-unmanaged.lua \
		$(1)/usr/sbin/openwisp-restore-unmanaged

	$(INSTALL_BIN) \
		files/sbin/openwisp-remove-default-wifi.lua \
		$(1)/usr/sbin/openwisp-remove-default-wifi

	$(INSTALL_BIN) \
		files/sbin/openwisp-uci-autoname.lua \
		$(1)/usr/sbin/openwisp-uci-autoname

	$(INSTALL_BIN) \
		files/sbin/openwisp-update-config.lua \
		$(1)/usr/sbin/openwisp-update-config

	$(INSTALL_BIN) \
		files/sbin/openwisp-get-address.lua \
		$(1)/usr/sbin/openwisp-get-address

	$(CP) ../VERSION $(1)/etc/openwisp/
endef

$(eval $(call BuildPackage,openwisp-config-openssl))
$(eval $(call BuildPackage,openwisp-config-mbedtls))
$(eval $(call BuildPackage,openwisp-config-cyassl))
$(eval $(call BuildPackage,openwisp-config-nossl))
