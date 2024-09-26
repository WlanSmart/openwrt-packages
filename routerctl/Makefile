include $(TOPDIR)/rules.mk

PKG_NAME:=routerctl
PKG_VERSION:=1.3.1
PKG_RELEASE:=1

PKG_MAINTAINER:=B <b@doubi.fun>

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL=git@github.com:WlanSmart/routerctl.git
PKG_SOURCE_VERSION=main

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
  CATEGORY:=App
  TITLE:=WlanSmart Router Control Program
  MENU:=1
  DEPENDS:=+libstdcpp
endef

define Package/$(PKG_NAME)/description
 WlanSmart Router Control Program
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR) CXX="$(TARGET_CXX)"
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/routerctl $(1)/usr/sbin/
	$(INSTALL_BIN) files/routerctl.init $(1)/etc/init.d/routerctl
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
