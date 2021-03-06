#
# Copyright (C) 2006 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
# $Id: Makefile,v 1.4 2011/01/18 08:49:41 satyabrat.sahu Exp $

include $(TOPDIR)/rules.mk

PKG_NAME:=rp-pppoe
PKG_VERSION:=3.8
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://roaringpenguin.com/files/download
PKG_MD5SUM:=0e32760f498f9cde44081ee6aafc823b

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL_DIR:=$(PKG_BUILD_DIR)/ipkg-install

include $(INCLUDE_DIR)/package.mk

define Package/rp-pppoe/Default
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+ppp
  TITLE:=PPPoE (PPP over Ethernet)
  URL:=http://roaringpenguin.com/penguin/pppoe/
  SUBMENU:=dial-in/up
endef

define Package/rp-pppoe-client
  $(call Package/rp-pppoe/Default)
  TITLE+= client
endef

define Package/rp-pppoe-relay
  $(call Package/rp-pppoe/Default)
  DEPENDS+= +cmm
  TITLE+= relay
endef

define Package/rp-pppoe-server
  $(call Package/rp-pppoe/Default)
  TITLE+= server
endef

define Package/rp-pppoe-sniff
  $(call Package/rp-pppoe/Default)
  TITLE+= sniffer
endef

TARGET_CFLAGS += -I$(PKG_BUILD_DIR)/src/libevent -I$(STAGING_DIR)/usr/include
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib
CONFIGURE_PATH := ./src
CONFIGURE_ARGS += ac_cv_path_PPPD=/usr/sbin/pppd
MAKE_FLAGS := RPM_INSTALL_ROOT="$(PKG_INSTALL_DIR)" install
MAKE_PATH := ./src

define Package/rp-pppoe-client/install	
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/pppoe-client.init $(1)/etc/init.d/pppoe-client
	$(INSTALL_DIR) $(1)/etc/ppp
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/etc/ppp/pppoe.conf $(1)/etc/ppp/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/pppoe $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/scripts/pppoe-connect $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/scripts/pppoe-start $(1)/usr/sbin/
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/scripts/pppoe-stop $(1)/usr/sbin/
	$(SED) 's,modprobe,insmod,g' $(1)/usr/sbin/pppoe-connect
endef

define Package/rp-pppoe-relay/install	
	$(CP) ./files/etc $(1)/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/pppoe-relay $(1)/usr/sbin/
ifeq ($(CONFIG_PACKAGE_webif),y)
	$(CP) ./files/www $(1)/
endif
endef

define Package/rp-pppoe-server/install	
	$(INSTALL_DIR) $(1)/etc/default
	$(INSTALL_BIN) ./files/pppoe-server.default $(1)/etc/default/pppoe-server
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/pppoe-server.init $(1)/etc/init.d/pppoe-server
	$(INSTALL_DIR) $(1)/etc/ppp
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/etc/ppp/pppoe-server-options $(1)/etc/ppp/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/pppoe-server $(1)/usr/sbin/
endef

define Package/rp-pppoe-sniff/install	
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/sbin/pppoe-sniff $(1)/usr/sbin/
endef

$(eval $(call BuildPackage,rp-pppoe-client))
$(eval $(call BuildPackage,rp-pppoe-relay))
$(eval $(call BuildPackage,rp-pppoe-server))
$(eval $(call BuildPackage,rp-pppoe-sniff))
