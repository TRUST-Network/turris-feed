#
# Copyright (C) 2019 CZ.NIC z.s.p.o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=atlas-probe
PKG_VERSION:=2.2.0
PKG_RELEASE:=1

PKG_SOURCE:=ripe-atlas-probe-busybox-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/RIPE-NCC/ripe-atlas-probe-busybox/archive/v$(PKG_VERSION)
PKG_HASH:=4fd1b5ace8218239d2395ef912080d22fad46a27ff7836b176d6634e79bfaa18

PKG_BUILD_DIR:=$(BUILD_DIR)/ripe-atlas-probe-busybox-$(PKG_VERSION)

PKG_MAINTAINER:=CZ.NIC <packaging@turris.cz>
PKG_LICENSE:=GPL-2.0-or-later
PKG_LICENSE_FILES:=LICENSE

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/atlas-probe
  SECTION:=net
  CATEGORY:=Network
  TITLE:=RIPE Atlas probe measurement
  DEPENDS:=+librt +libopenssl +openssh-client +sudo
  USERID:=atlas=444:atlas=444
  URL:=https://atlas.ripe.net/
endef

define Package/atlas-probe/description
  RIPE Atlas is a global, open, distributed Internet measurement platform,
  consisting of thousands of measurement devices that measure Internet
  connectivity in real time.
endef

TARGET_CFLAGS += $(FPIC)

CONFIGURE_ARGS += \
	--disable-shared \
	--enable-static

CONFIGURE_PATH = libevent-2.1.11-stable

TARGET_LDFLAGS = -L$(PKG_BUILD_DIR)/$(CONFIGURE_PATH)/.libs

define Build/Compile
	+$(MAKE_VARS) \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(CONFIGURE_PATH) \
		$(MAKE_FLAGS)
	+$(MAKE_VARS) \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		$(MAKE_FLAGS)
endef

define Package/atlas-probe/install
	+$(MAKE_VARS) \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR) \
		$(MAKE_FLAGS) CONFIG_PREFIX=$(1)/usr/libexec/atlas-probe install
	$(INSTALL_DIR) $(1)/usr/libexec/atlas-probe/state
	echo $(PKG_VERSION) > $(1)/usr/libexec/atlas-probe/state/VERSION
endef

$(eval $(call BuildPackage,atlas-probe))
