#
# Copyright (C) 2019-2020 CZ.NIC z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v3.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=foris-controller-schnapps-module
PKG_VERSION:=0.4
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://gitlab.nic.cz/turris/foris-controller/foris-controller-schnapps-module.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)

PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>
PKG_LICENSE:=GPL-3.0-or-later
PKG_LICENSE_FILES:=LICENSE

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/python/python3-package.mk
include ../foris-controller/files/foris-controller-module.mk

define Package/foris-controller-schnapps-module
  TITLE:=Foris-controller schnapps module
  DEPENDS:= \
    +foris-controller \
    +schnapps
  VARIANT:=python3
endef

define Package/foris-controller-schnapps-module/description
  This is a schnapps module for foris-controller.
endef

$(eval $(call ForisControllerModule,foris-controller-schnapps-module))
# call BuildPackage <= assures OWR build system that this is package
