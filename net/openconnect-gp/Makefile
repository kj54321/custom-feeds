#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=openconnect-gp
PKG_VERSION:=8.00
PKG_RELEASE:=1
PKG_USE_MIPS16:=0

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/dlenski/openconnect.git
PKG_SOURCE_VERSION:=c62a909a7793d9eab00c96370dc3884a5d32b471
PKG_LICENSE:=LGPLv2.1+

PKG_CONFIG_DEPENDS:= \
	CONFIG_OPENCONNECT_GNUTLS \
	CONFIG_OPENCONNECT_OPENSSL \

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_INSTALL:=1
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/openconnect-gp/config
	source "$(SOURCE)/Config.in"
endef

define Package/openconnect-gp
  SECTION:=net
  CATEGORY:=Network
  DEPENDS:=+libxml2 +kmod-tun +resolveip +vpnc-scripts +OPENCONNECT_OPENSSL:libopenssl +OPENCONNECT_OPENSSL:p11-kit +OPENCONNECT_OPENSSL:libp11 +OPENCONNECT_GNUTLS:libgnutls +OPENCONNECT_STOKEN:libstoken
  TITLE:=OpenConnect VPN client - GLobalProtect
  MAINTAINER:=dlenski
  URL:=https://github.com/dlenski/openconnect
  SUBMENU:=VPN
  CONFLICTS:=openconnect
endef

define Package/openconnect-gp/description
	A VPN client compatible with Cisco's AnyConnect SSL VPN, ocserv and Juniper (Pulse secure).

        OpenConnect is a client that follows the Cisco's AnyConnect SSL VPN protocol,
        which is supported by IOS 12.4(9)T or later on Cisco SR500, 870, 880, 1800,
        2800, 3800, 7200 Series and Cisco 7301 Routers, as well as the OpenConnect
        VPN server. It has later been ported to support the Juniper SSL VPN which
        is now known as Pulse Connect Secure.
endef

#define Build/Configure
#	( cd $(PKG_BUILD_DIR); ./autogen.sh )
#	$(call Build/Configure/Default)
#endef

CONFIGURE_ARGS += \
	--disable-shared \
	--with-vpnc-script=/lib/netifd/vpnc-script \
	--without-libpcsclite \
	--without-stoken \
	--without-libpskc \
	--without-gssapi \
	--without-lz4

ifeq ($(CONFIG_OPENCONNECT_OPENSSL),y)
CONFIGURE_ARGS += \
	--without-gnutls
endif

ifeq ($(CONFIG_OPENCONNECT_STOKEN),y)
CONFIGURE_ARGS += \
	--with-stoken
endif

define Package/openconnect-gp/install
	$(INSTALL_DIR) $(1)/etc/openconnect/
	$(INSTALL_DIR) $(1)/lib/netifd/proto
	$(INSTALL_BIN) ./files/openconnect.sh $(1)/lib/netifd/proto/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/openconnect $(1)/usr/sbin/
	$(INSTALL_BIN) ./files/openconnect-wrapper $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/lib/upgrade/keep.d
	$(INSTALL_DATA) ./files/openconnect.upgrade $(1)/lib/upgrade/keep.d/openconnect
endef

$(eval $(call BuildPackage,openconnect-gp))
