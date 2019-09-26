#
# Copyright (C) 2006-2019 Andreas Nilsen <adde88@gmail.com>
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=asleap
PKG_VERSION:=2.2
PKG_RELEASE:=4

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com>

include $(INCLUDE_DIR)/package.mk

define Package/asleap
	SECTION:=base
	CATEGORY:=Network
	TITLE:=Actively recover LEAP/PPTP passwords
	URL:=https://tools.kali.org/wireless-attacks/asleap
	DEPENDS:=+libpthread +libopenssl +libpcap
endef

define Package/asleap/description
  LEAP utilizes a modified MS-CHAPv2 challenge/response in order to
  authenticate users on a wireless network.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) -rf ./src/* $(PKG_BUILD_DIR)/
endef

define Build/RunMake
	CFLAGS="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
	$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(1) \
		$(TARGET_CONFIGURE_OPTS) \
		LIBS="$(TARGET_LDFLAGS)" \
		LIBS_c="$(TARGET_LDFLAGS_C)" \
		BCHECK= \
		$(2)
endef

define Package/asleap/install
		$(INSTALL_DIR) $(1)/usr/sbin
		$(INSTALL_BIN) $(PKG_BUILD_DIR)/asleap $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,asleap))
