include $(TOPDIR)/rules.mk

PKG_NAME:=ath10k-ct
PKG_VERSION:=2016-12-09
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/greearb/ath10k-ct.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=76533a62824bddfc829ec6ec7cc2a26e9db3d3a2
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.xz

PKG_MAINTAINER:=Ben Greear <greearb@candelatech.com>
PKG_BUILD_PARALLEL:=1

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/ath10k-ct
  SUBMENU:=Wireless Drivers
  TITLE:=ath10k-ct driver optimized for CT ath10k firmware
  DEPENDS:=+kmod-mac80211 +kmod-cfg80211 +kmod-thermal +kmod-ath +@DRIVER_11N_SUPPORT @PCI_SUPPORT +@DRIVER_11AC_SUPPORT +@KERNEL_RELAY
  FILES:=\
	$(PKG_BUILD_DIR)/ath10k/ath10k_pci.ko \
	$(PKG_BUILD_DIR)/ath10k/ath10k_core.ko
  AUTOLOAD:=$(call AutoLoad,50,compat cfg80211 mac80211 ath ath10k_core ath10k_pci)
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-include backport/autoconf.h \
	-include backport/backport.h

ifdef CONFIG_PACKAGE_MAC80211_MESH
  NOSTDINC_FLAGS += -DCONFIG_MAC80211_MESH
endif

CT_MAKEDEFS += CONFIG_ATH10K=m CONFIG_ATH10K_PCI=m CONFIG_ATH10K_DFS_CERTIFIED=y CONFIG_CFG80211_CERTIFICATION_ONUS=y
# No AHB support enabled yet.  Could conditionally enable it later.
#CT_MAKEDEFS += CONFIG_ATH10K_AHB=y
#NOSTDINC_FLAGS += -DCONFIG_ATH10K_AHB
NOSTDINC_FLAGS += -DSTANDALONE_CT

ifdef CONFIG_PACKAGE_MAC80211_DEBUGFS
  CT_MAKEDEFS += CONFIG_ATH10K_DEBUGFS=y CONFIG_MAC80211_DEBUGFS=y
  NOSTDINC_FLAGS += -DCONFIG_MAC80211_DEBUGFS
  NOSTDINC_FLAGS += -DCONFIG_ATH10K_DEBUGFS
endif

ifdef CONFIG_PACKAGE_ATH_DEBUG
  NOSTDINC_FLAGS += -DCONFIG_ATH10K_DEBUG
endif

ifdef CONFIG_PACKAGE_ATH_DFS
  NOSTDINC_FLAGS += -DCONFIG_ATH10K_DFS_CERTIFIED
endif

define Build/Configure
	cp $(STAGING_DIR)/usr/include/mac80211/ath/*.h $(PKG_BUILD_DIR)
endef

ifneq ($(findstring c,$(OPENWRT_VERBOSE)),)
  CT_MAKEDEFS += V=1
endif

define Build/Compile
	+$(MAKE) $(CT_MAKEDEFS) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		ARCH="$(LINUX_KARCH)" \
		CROSS_COMPILE="$(TARGET_CROSS)" \
		SUBDIRS="$(PKG_BUILD_DIR)/ath10k" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,ath10k-ct))
