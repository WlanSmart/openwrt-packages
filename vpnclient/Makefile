include $(TOPDIR)/rules.mk

PKG_NAME:=vpnclient
PKG_VERSION:=0.0.1
PKG_RELEASE:=1

PKG_MAINTAINER:=B <b@doubi.fun>

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL=git@github.com:abxuz/vpnclient.git
PKG_SOURCE_VERSION=main

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=VCloud VPN Client
  MENU:=1
  DEPENDS:=+libstdcpp +libopenssl
endef

define Package/$(PKG_NAME)/description
 VCloud VPN Client
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) CXX="$(TARGET_CXX)"
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/vcloud $(1)/usr/sbin/
	$(INSTALL_BIN) files/vcloud.init $(1)/etc/init.d/vcloud
	$(INSTALL_CONF) files/vcloud.conf $(1)/etc/config/vcloud
endef

$(eval $(call BuildPackage,$(PKG_NAME)))