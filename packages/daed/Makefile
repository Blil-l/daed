include $(TOPDIR)/rules.mk

PKG_NAME:=daed
PKG_VERSION:=2.1.1
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/Blil-l/daed.git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_MIRROR_HASH:=skip

PKG_LICENSE:=AGPL-3.0-only MIT
PKG_MAINTAINER:=Blil-l

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)/wing
PKG_BUILD_DEPENDS:=golang/host bpf-headers
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/daeuniverse/dae-wing
GO_PKG_TAGS:=embedallowed,trace
GO_PKG_LDFLAGS:= \
    -X '$(GO_PKG)/db.AppDescription=$(PKG_NAME) is a integration solution of dae, API and UI.'
GO_PKG_LDFLAGS_X= \
    $(GO_PKG)/db.AppName=$(PKG_NAME) \
    $(GO_PKG)/db.AppVersion=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/bpf.mk
include ../../lang/golang/golang-package.mk

define Package/daed
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Web Servers/Proxies
  TITLE:=A Modern Dashboard For dae
  DEPENDS:=$(GO_ARCH_DEPENDS) $(BPF_DEPENDS) +ca-bundle +vmlinux-btf
  URL:=https://github.com/daeuniverse/daed
endef

define Package/daed/description
  daed is a backend of dae, provides a method to bundle arbitrary frontend, dae
  and geodata into one binary.
endef

define Package/daed/conffiles
  /etc/daed/wing.db
  /etc/config/daed
endef

define Build/Compile
  $(call GoPackage/Build/Compile)
endef

define Package/daed/install
  $(INSTALL_DIR) $(1)/usr/bin
  $(INSTALL_BIN) $(PKG_BUILD_DIR)/daed $(1)/usr/bin/
  $(INSTALL_DIR) $(1)/etc/daed
  $(INSTALL_DIR) $(1)/etc/config
  $(INSTALL_DIR) $(1)/etc/init.d
  $(INSTALL_BIN) ./files/daed.init $(1)/etc/init.d/daed
  $(INSTALL_CONF) ./files/daed.config $(1)/etc/config/daed
endef

$(eval $(call BuildPackage,daed))
