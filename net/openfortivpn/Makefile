include $(TOPDIR)/rules.mk

PKG_NAME:=openfortivpn
PKG_VERSION:=1.10.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/adrienverge/openfortivpn.git
PKG_SOURCE_VERSION:=f7f3843c71017b788fb0f92d98bedda472bdba15

PKG_LICENSE:=GPL-3.0-or-later OpenSSL
PKG_LICENSE_FILES:=LICENSE LICENSE.OpenSSL
PKG_MAINTAINER:=Donny Jie <dong115@uwindsor.ca>
PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/openfortivpn
  SUBMENU:=VPN
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Fortinet SSL VPN client
  URL:=https://github.com/adrienverge/openfortivpn
  DEPENDS:=+ppp +libopenssl
endef

define Package/openfortivpn/description
  An open implementation of Fortinet's proprietary PPP+SSL VPN solution
  It spawns a pppd process and operates the communication between the gateway and this process.
  It is compatible with Fortinet VPNs.
endef

CONFIGURE_ARGS += \
	--enable-proc \
	--with-rt_dst="yes" \
	--with-pppd="/usr/sbin/pppd"

TARGET_LDFLAGS += -Wl,--gc-sections,--as-needed

define Package/openfortivpn/conffiles
/etc/config/openfortivpn
endef

define Package/openfortivpn/install
	$(INSTALL_DIR) \
	  $(1)/usr/sbin \
	  $(1)/etc/config \
	  $(1)/etc/init.d

	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/openfortivpn $(1)/usr/sbin/
	$(INSTALL_DATA) ./files/openfortivpn.config $(1)/etc/config/openfortivpn
	$(INSTALL_BIN) ./files/openfortivpn.init $(1)/etc/init.d/openfortivpn
endef

$(eval $(call BuildPackage,openfortivpn))
