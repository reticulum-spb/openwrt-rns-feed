include $(TOPDIR)/rules.mk

PKG_NAME:=rs-nodepage
PKG_VERSION:=0.1.0
PKG_RELEASE:=1

PKG_SOURCE_VERSION:=a8021bdf04e66f2f8e9c96f2f75579228fab4058
PKG_SOURCE:=rsNodePage-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/reticulum-spb/rsNodePage/tar.gz/$(PKG_SOURCE_VERSION)?
PKG_HASH:=59f79299fc8acc5b287ae98a874eeb09c0de8e14d60271724d9775cecb56c4d5

RSRETICULUM_VERSION:=5e85d82b53f9c299caf378683294dfeedfdd029f
RSRETICULUM_SOURCE:=rsReticulum-$(RSRETICULUM_VERSION).tar.gz

PKG_MAINTAINER:=RNS Gate
PKG_LICENSE:=AGPL-3.0-or-later
PKG_BUILD_DEPENDS:=$(if $(RNS_EXTERNAL_RUST),,rust/host)
PKG_BUILD_PARALLEL:=1
PKG_BUNDLE_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_BUILD_DIR:=$(PKG_BUNDLE_DIR)/rsNodePage
PKG_UNPACK=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components=1 -xzf $(DL_DIR)/$(PKG_SOURCE)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/rust/rust-package.mk

CARGO_PKG_PROFILE:=release
CARGO_PKG_CONFIG_VARS += CARGO_TARGET_DIR=$(BUILD_DIR)/rns-cargo-target

define Download/rsreticulum
  URL:=https://codeload.github.com/reticulum-spb/rsReticulum/tar.gz/$(RSRETICULUM_VERSION)?
  FILE:=$(RSRETICULUM_SOURCE)
  HASH:=f70c019daaf0108733d9b4fbf8c979edfbcef4dac0b40d4581946f48f47422ff
endef

define Package/rs-nodepage
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NodePage server in Rust
  DEPENDS:=$(RUST_ARCH_DEPENDS)
  URL:=https://github.com/reticulum-spb/rsNodePage
endef

define Package/rs-nodepage/description
  Rust command-line implementation of the NodePage server.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(eval $(call Download,rsreticulum))
	rm -rf $(PKG_BUNDLE_DIR)/rsReticulum
	mkdir -p $(PKG_BUNDLE_DIR)/rsReticulum
	$(TAR) -xf $(DL_DIR)/$(RSRETICULUM_SOURCE) --strip=1 -C $(PKG_BUNDLE_DIR)/rsReticulum
	$(SED) 's/as nix::libc::c_ulong;/as nix::libc::c_int;/' \
		$(PKG_BUNDLE_DIR)/rsReticulum/crates/rns-runtime/src/rnsh.rs
endef

define Build/Compile
	$(call Build/Compile/Cargo,crates/nomad-tools)
endef

$(eval $(call RustBinPackage,rs-nodepage))
$(eval $(call BuildPackage,rs-nodepage))
