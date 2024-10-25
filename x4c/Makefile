include $(TOPDIR)/rules.mk

PKG_NAME:=x4c
PKG_VERSION:=1.0.1
PKG_RELEASE:=1

PKG_MAINTAINER:=B <b@doubi.fun>

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL=git@github.com:abxuz/x4c.git
PKG_SOURCE_VERSION=main

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  CATEGORY:=App
  TITLE:=XVPN Client by C++
  MENU:=1
  DEPENDS:=+libstdcpp +libopenssl
endef

define Package/$(PKG_NAME)/description
 XVPN Client by C++
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) CC="$(TARGET_CXX)"
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/x4c $(1)/usr/sbin/
	$(INSTALL_BIN) files/x4c.init $(1)/etc/init.d/x4c
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
