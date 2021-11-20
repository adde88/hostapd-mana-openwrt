#
# Copyright (C) 2021 Andreas Nilsen <adde88@gmail.com> https://github.com/adde88
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=hostapd-mana
PKG_VERSION:=2.6.5
PKG_RELEASE:=30

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)

PKG_MAINTAINER:=Andreas Nilsen <adde88@gmail.com>
PKG_LICENSE:=GPL-2.0
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_PARALLEL:=1
PKG_ASLR_PIE_REGULAR:=1

PKG_CONFIG_DEPENDS:= \
	CONFIG_PACKAGE_kmod-ath9k \
	CONFIG_PACKAGE_kmod-cfg80211 \
	CONFIG_PACKAGE_hostapd \
	CONFIG_PACKAGE_hostapd-basic \
	CONFIG_PACKAGE_hostapd-mini \
	CONFIG_MANA_RFKILL_SUPPORT \
	CONFIG_MANA_MSG_MIN_PRIORITY \
	CONFIG_MANA_DRIVER_WEXT_SUPPORT \
	CONFIG_MANA_DRIVER_11N_SUPPORT \
	CONFIG_MANA_DRIVER_11AC_SUPPORT \
#	CONFIG_MANA_DRIVER_11AX_SUPPORT \
	CONFIG_MANA_DRIVER_TAXONOMY_SUPPORT

include $(INCLUDE_DIR)/package.mk

STAMP_CONFIGURED:=$(STAMP_CONFIGURED)_$(CONFIG_MANA_MSG_MIN_PRIORITY)

ifneq ($(CONFIG_MANA_DRIVER_11N_SUPPORT),)
  HOSTAPD_IEEE80211N:=y
endif

ifneq ($(CONFIG_MANA_DRIVER_11AC_SUPPORT),)
  HOSTAPD_IEEE80211AC:=y
endif

#ifneq ($(CONFIG_MANA_DRIVER_11AX_SUPPORT),)
#  HOSTAPD_IEEE80211AX:=y
#endif

ifneq ($(CONFIG_MANA_DRIVER_TAXONOMY_SUPPORT),)
  CONFIG_MANA_DRIVER_TAXONOMY_SUPPORT:=y
endif

DRIVER_MAKEOPTS= \
	CONFIG_ACS=$(CONFIG_PACKAGE_kmod-cfg80211) \
	CONFIG_DRIVER_NL80211=$(CONFIG_PACKAGE_kmod-cfg80211) \
	CONFIG_IEEE80211N=$(HOSTAPD_IEEE80211N) \
	CONFIG_IEEE80211AC=$(HOSTAPD_IEEE80211AC) \
#	CONFIG_IEEE80211AX=$(HOSTAPD_IEEE80211AX) \
	CONFIG_DRIVER_WEXT=$(CONFIG_MANA_DRIVER_WEXT_SUPPORT) \
	CONFIG_TAXONOMY=$(CONFIG_MANA_DRIVER_TAXONOMY_SUPPORT) \

ifdef CONFIG_MANA_RFKILL_SUPPORT
  DRIVER_MAKEOPTS += NEED_RFKILL=y
endif

ifdef CONFIG_USE_GLIBC
  TARGET_LDFLAGS += -lrt
  TARGET_LDFLAGS_C += -lrt
endif

define Package/hostapd-mana/config
  source "$(SOURCE)/Config.in"
endef

define Package/hostapd-mana
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=WirelessAPD
  TITLE:=Hostapd with support for the MANA and Karma attacks.
  DEPENDS:=$(DRV_DEPENDS) +hostapd-common +libubus +libopenssl +libnl-tiny +getopt
  URL:=https://github.com/sensepost/hostapd-mana
  PROVIDES:=hostapd-mana
  CONFLICTS:=$(HOSTAPD_PROVIDERS)
  HOSTAPD_PROVIDERS+=$(1)
endef

define Package/hostapd-mana/description
  This package contains hostapd with support for the MANA/KARMA attacks.
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) -rf ./src/* $(PKG_BUILD_DIR)/
	$(call Build/Prepare/Default)
endef

ifneq ($(wildcard $(PKG_BUILD_DIR)/.config_*),$(subst .configured_,.config_,$(STAMP_CONFIGURED)))
  define Build/Configure/rebuild
	$(FIND) $(PKG_BUILD_DIR) -name \*.o -or -name \*.a | $(XARGS) rm -f
	rm -f $(PKG_BUILD_DIR)/hostapd/hostapd
	rm -f $(PKG_BUILD_DIR)/.config_*
	touch $(subst .configured_,.config_,$(STAMP_CONFIGURED))
  endef
endif

define Build/Configure
	$(Build/Configure/rebuild)
	$(if $(wildcard ./files/hostapd-mana.config), \
		$(CP) ./files/hostapd-mana.config $(PKG_BUILD_DIR)/hostapd/.config \
	)
endef

TARGET_CPPFLAGS := \
	-I$(STAGING_DIR)/usr/include/libnl-tiny \
	-I$(PKG_BUILD_DIR)/src/crypto \
	$(TARGET_CPPFLAGS) \
	-DCONFIG_LIBNL20 \
	-D_GNU_SOURCE \
	$(if $(CONFIG_MANA_MSG_MIN_PRIORITY),-DCONFIG_MSG_MIN_PRIORITY=$(CONFIG_MANA_MSG_MIN_PRIORITY))

TARGET_CFLAGS += -ffunction-sections -fdata-sections -flto
TARGET_LDFLAGS += -Wl,--gc-sections -flto=jobserver -fuse-linker-plugin -lubox -lubus -lm -lnl-tiny -lcrypto -lssl

ifdef CONFIG_MANA_WPA_ENABLE_WEP
  DRIVER_MAKEOPTS += CONFIG_WEP=y
endif

define Build/RunMake
        CFLAGS="$(TARGET_CPPFLAGS) $(TARGET_CFLAGS)" \
        $(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/$(1) \
                $(TARGET_CONFIGURE_OPTS) \
                $(DRIVER_MAKEOPTS) \
                LIBS="$(TARGET_LDFLAGS)" \
                LIBS_c="$(TARGET_LDFLAGS_C)" \
		AR="$(TARGET_CROSS)gcc-ar" \
                BCHECK= \
		$(if $(findstring s,$(OPENWRT_VERBOSE)),V=1) \
                $(2)
endef

define Build/Compile/hostapd-mana
	+$(call Build/RunMake,hostapd, \
		hostapd hostapd_cli \
	)
endef

define Build/Compile
	@echo "-----------------------------------------"
	@echo "-                                        "
	@echo "-  COMPILING: $(PKG_NAME)-$(PKG_VERSION) "
	@echo "-  ARCHITECTURE: $(ARCH)                 "
	@echo "-  BOARD: $(BOARD)                       "
	@echo "-  SUBTARGET: $(SUBTARGET)               "
	@echo "-                                        "
	@echo "-----------------------------------------"
	sleep 4
	$(Build/Compile/hostapd-mana)
	$(Build/Compile/full)
endef

define Install/hostapd-mana
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/hostapd-mana/
	$(INSTALL_DIR) $(1)/etc/hostapd-mana/cert
endef

define Package/hostapd-mana/install
	$(call Install/hostapd-mana,$(1))
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd		$(1)/usr/sbin/hostapd-mana
	$(INSTALL_BIN)  $(PKG_BUILD_DIR)/hostapd/hostapd_cli		$(1)/usr/sbin/mana_cli
	$(INSTALL_BIN)	$(PKG_BUILD_DIR)/files/crackapd/crackapd.py	$(1)/usr/sbin/crackapd
	$(INSTALL_BIN)	$(PKG_BUILD_DIR)/files/berate_ap		$(1)/usr/sbin/berate_ap
	$(INSTALL_DATA)	$(PKG_BUILD_DIR)/files/crackapd/crackapd.conf	$(1)/etc/hostapd-mana/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/conf/*			$(1)/etc/hostapd-mana/
	$(INSTALL_DATA) $(PKG_BUILD_DIR)/files/cert/*			$(1)/etc/hostapd-mana/cert/
endef

$(eval $(call BuildPackage,hostapd-mana))
