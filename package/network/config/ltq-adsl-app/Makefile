#
# Copyright (C) 2011-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=dsl_cpe_control_danube
PKG_VERSION:=3.24.4.4
PKG_RELEASE:=15
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/dsl_cpe_control-$(PKG_VERSION)
PKG_SOURCE_URL:=@OPENWRT
PKG_HASH:=af0bdf45cc7a62e2b38d39aad4924dd83c24fae170ae5bbd8190c2a3d9106257
PKG_MAINTAINER:=John Crispin <john@phrozen.org>
PKG_LICENSE:=BSD-3-Clause

PKG_FIXUP:=autoreconf

PKG_CONFIG_DEPENDS:=\
	CONFIG_LTQ_DSL_ENABLE_SOAP \
	CONFIG_LTQ_DSL_ENABLE_DSL_EVENT_POLLING

PKG_BUILD_DEPENDS:=ltq-adsl

PKG_FLAGS:=nonshared

include $(INCLUDE_DIR)/package.mk

define Package/ltq-adsl-app
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Lantiq DSL userland tool
  URL:=http://www.lantiq.com/
  DEPENDS:=@(TARGET_lantiq_xway||TARGET_lantiq_xway_legacy||TARGET_lantiq_ase) +libpthread +ltq-dsl-base +libubox +libubus
  PROVIDES:=ltq-dsl-app
  MENU:=1
endef

define Package/ltq-adsl-app/description
	Infineon DSL CPE API for Amazon SE, Danube and Vinax.
endef

LTQ_DSL_MAX_DEVICE=1
LTQ_DSL_LINES_PER_DEVICE=1
LTQ_DSL_CHANNELS_PER_LINE=1

CONFIGURE_ARGS += \
	--with-max-device="$(LTQ_DSL_MAX_DEVICE)" \
	--with-lines-per-device="$(LTQ_DSL_LINES_PER_DEVICE)" \
	--with-channels-per-line="$(LTQ_DSL_CHANNELS_PER_LINE)" \
	--enable-danube \
	--enable-driver-include="-I$(STAGING_DIR)/usr/include/adsl/" \
	--enable-add-appl-cflags="-DMAX_CLI_PIPES=2" \
	--enable-model=full \
	--disable-dsl-pm-retx-counters \
	--disable-dsl-pm-retx-thresholds \
	--disable-soap-support \
	--disable-dti \
	--disable-adsl-mib-support \
	--disable-adsl-trace \
	--enable-adsl-led

TARGET_CFLAGS += -I$(LINUX_DIR)/include

define Build/Prepare
	$(call Build/Prepare/Default)
	$(CP) ../ltq-vdsl-vr9-app/src/src/dsl_cpe_ubus.c $(PKG_BUILD_DIR)/src/
endef

define Package/ltq-adsl-app/install
	$(INSTALL_DIR) $(1)/etc/init.d $(1)/sbin $(1)/etc/hotplug.d/dsl
	$(INSTALL_BIN) ./files/dsl_control $(1)/etc/init.d/
	$(INSTALL_BIN) ./files/10_atm.sh $(1)/etc/hotplug.d/dsl
	$(INSTALL_BIN) ./files/10_ptm.sh $(1)/etc/hotplug.d/dsl
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/dsl_cpe_control $(1)/sbin
	$(INSTALL_BIN) ./files/dsl_cpe_pipe.sh $(1)/sbin/
endef

$(eval $(call BuildPackage,ltq-adsl-app))
