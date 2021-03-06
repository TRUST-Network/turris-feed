#
## Copyright (C) 2013-2015 CZ.NIC z.s.p.o. (http://www.nic.cz/)
#
## This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
# #
#
include $(TOPDIR)/rules.mk

PKG_NAME:=cznic-cacert-bundle
PKG_VERSION:=38
PKG_RELEASE:=3

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://gitlab.nic.cz/turris/misc.git
PKG_SOURCE_VERSION:=1496ca959d1b51c2e98d623363e1fd9eba464d27

PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>

include $(INCLUDE_DIR)/package.mk

define Package/cznic-cacert-bundle/postinst
#!/bin/sh

[ -n "$$IPKG_INSTROOT" ] || {
	# TODO: If ever anything gets removed from this list, use -r, not delete it manually.
	/usr/sbin/cert-backup \
		/etc/ssl/turris.pem \
		/usr/bin/get-api-crl \
		/etc/ssl/ucollect-server.pem \
		-r /etc/ssl/www_turris_cz_ca.pem \
		-r /etc/ssl/startcom.pem \
		-r /etc/ssl/api.turris.pem
	get-api-crl
}
endef

define Package/cznic-cacert-bundle
	TITLE:=CZNIC cacert bundle
	DEPENDS:=+cert-backup +c-rehash
endef

# The built is empty. But as there's no makefile in the git repo, we need to
# override the default that runs "make".
define Build/Compile
	true
endef

define Package/cznic-cacert-bundle/install
	$(INSTALL_DIR) $(1)/etc/ssl/
	# TODO: If anything is modified here, it needs to be updated in the postinst too
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/cacerts/emergency-ca/ca.crt $(1)/etc/ssl/turris.pem
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/cacerts/ucollect-server.pem $(1)/etc/ssl/
	ln -s /tmp/crl.pem $(1)/etc/ssl/crl.pem

	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/cacerts/get-api-crl $(1)/usr/bin/

	$(INSTALL_DIR) $(1)/etc/cron.d
	$(INSTALL_DATA) ./files/get-crl.cron $(1)/etc/cron.d/get-api
endef

$(eval $(call BuildPackage,cznic-cacert-bundle))
