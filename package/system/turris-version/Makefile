#
## Copyright (C) 2014-2015 CZ.NIC z.s.p.o. (http://www.nic.cz/)
#
## This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# #
#
include $(TOPDIR)/rules.mk

PKG_NAME:=turris-version
PKG_VERSION:=3.7

PKG_MAINTAINER:=Michal Vaner <michal.vaner@nic.cz>

include $(INCLUDE_DIR)/package.mk

define Package/turris-version
	TITLE:=turris-version
endef

define Build/Prepare
endef

define Build/Compile
endef

define Package/turris-version/postinst
#!/bin/sh
# Danger: spaces are not ordinary spaces, but special unicode ones
[ -n "$$IPKG_INSTROOT" ] || {
create_notification -s news " • foris: URL změněno na /foris
 • foris: jednoduché nastavení guest WiFi
 • foris: slovenský a polský překlad
 • foris: více možností u VPN
 • updater: migrace Turris 1.0 a 1.1 na nový updater" " • foris: URL changed to /foris
 • foris: simple guest WiFi setup
 • foris: Slovak and Polish translation
 • foris: more options for OpenVPN server
 • updater: migration to new updater on Turris router 1.0 and 1.1"
}
endef

define Package/turris-version/install
	$(INSTALL_DIR) $(1)/etc
	echo $(PKG_VERSION) > $(1)/etc/turris-version
endef

$(eval $(call BuildPackage,turris-version))
