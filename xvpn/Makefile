include $(TOPDIR)/rules.mk

PKG_NAME:=xvpn
PKG_VERSION:=0.0.1
PKG_RELEASE:=1

PKG_MAINTAINER:=B <b@doubi.fun>

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL=git@git.doubi.fun:ab/xvpn.git
PKG_SOURCE_VERSION=main

PKG_CONFIG_DEPENDS:= \
	CONFIG_XVPN_GO_COMPRESS_UPX

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=xvpn
GO_PKG_LDFLAGS:=-s -w

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  DEPENDS:=$$(GO_ARCH_DEPENDS)
  TITLE:=XVpn Cli Client
endef

define Package/$(PKG_NAME)/description
 XVpn Cli Client
endef


define Package/$(PKG_NAME)/config
menu "Configuration"
	depends on PACKAGE_$(PKG_NAME)

config XVPN_GO_COMPRESS_UPX
	bool "Compress executable files with UPX"
	default y

endmenu
endef

define Build/compile
	$(call GoPackage/Build/Compile)
ifeq ($(CONFIG_XVPN_GO_COMPRESS_UPX),y)
	wget -q https://github.com/upx/upx/releases/download/v4.1.0/upx-4.1.0-amd64_linux.tar.xz -O $(DL_DIR)/upx-4.1.0-amd64_linux.tar.xz
	rm -rf $(PKG_bBUILD_DIR)/upx
	mkdir -p $(BUILD_DIR)/upx
	xz -d -c $(DL_DIR)/upx-4.1.0-amd64_linux.tar.xz | tar -x -C $(BUILD_DIR)/upx
	chmod +x $(BUILD_DIR)/upx/upx-4.1.0-amd64_linux/upx
	$(BUILD_DIR)/upx/upx-4.1.0-amd64_linux/upx --lzma --best $(GO_PKG_BUILD_BIN_DIR)/$(PKG_NAME)
endif
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_BIN) $(GO_PKG_BUILD_BIN_DIR)/$(PKG_NAME) $(1)/usr/sbin/
	$(CP) files/xvpn.init $(1)/etc/init.d/xvpn
	$(CP) files/xvpn.config $(1)/etc/config/xvpn
endef

$(eval $(call BuildPackage,$(PKG_NAME)))