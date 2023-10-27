include $(TOPDIR)/rules.mk

PKG_NAME:=xvpn
PKG_VERSION:=0.0.2
PKG_RELEASE:=1

PKG_MAINTAINER:=B <b@doubi.fun>

PKG_SOURCE_PROTO=git
PKG_SOURCE_URL=git@github.com:abxuz/xvpn.git
PKG_SOURCE_VERSION=v0.0.2

PKG_CONFIG_DEPENDS:= \
	CONFIG_XVPN_GO_COMPRESS_UPX

include $(INCLUDE_DIR)/package.mk
include golang-values.mk

define Package/$(PKG_NAME)
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=VPN
  TITLE:=XVpn Cli Client
  MENU:=1
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

define Build/Prepare
	$(Build/Prepare/Default)
	wget -q https://go.dev/dl/go1.21.3.linux-amd64.tar.gz -O $(DL_DIR)/go1.21.3.linux-amd64.tar.gz
	rm -rf $(BUILD_DIR)/go1.21.3
	mkdir -p $(BUILD_DIR)/go1.21.3
	tar zxf $(DL_DIR)/go1.21.3.linux-amd64.tar.gz -C $(BUILD_DIR)/go1.21.3 --strip-components=1
endef

define Build/Compile
	cd $(PKG_BUILD_DIR) && \
	CGO_ENABLED=0 \
	GOOS="$(GO_OS)" \
	GOARCH="$(GO_ARCH)" \
	GO386="$(GO_386)" \
	GOAMD64="$(GO_AMD64)" \
	GOARM="$(GO_ARM)" \
	GOMIPS="$(GO_MIPS)" \
	GOMIPS64="$(GO_MIPS64)" \
	GOPPC64="$(GO_PPC64)" \
	GOCACHE="$(GO_BUILD_CACHE_DIR)" \
	GOMODCACHE="$(GO_MOD_CACHE_DIR)" \
	$(BUILD_DIR)/go1.21.3/bin/go build -v -buildvcs=false -trimpath -ldflags "-s -w" -trimpath -o $(PKG_BUILD_DIR)/$(PKG_NAME) .

ifeq ($(CONFIG_XVPN_GO_COMPRESS_UPX),y)
	wget -q https://github.com/upx/upx/releases/download/v4.1.0/upx-4.1.0-amd64_linux.tar.xz -O $(DL_DIR)/upx-4.1.0-amd64_linux.tar.xz
	rm -rf $(BUILD_DIR)/upx
	mkdir -p $(BUILD_DIR)/upx
	xz -d -c $(DL_DIR)/upx-4.1.0-amd64_linux.tar.xz | tar -x -C $(BUILD_DIR)/upx
	chmod +x $(BUILD_DIR)/upx/upx-4.1.0-amd64_linux/upx
	$(BUILD_DIR)/upx/upx-4.1.0-amd64_linux/upx --lzma --best $(PKG_BUILD_DIR)/$(PKG_NAME)
endif
endef

define Package/$(PKG_NAME)/install
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/$(PKG_NAME) $(1)/usr/sbin/
	$(INSTALL_BIN) files/xvpn.init $(1)/etc/init.d/xvpn
	$(CP) files/xvpn.config $(1)/etc/config/xvpn
endef

$(eval $(call BuildPackage,$(PKG_NAME)))