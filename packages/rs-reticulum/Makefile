include $(TOPDIR)/rules.mk

PKG_NAME:=rs-reticulum
PKG_VERSION:=1.5.2
PKG_RELEASE:=2

PKG_SOURCE_VERSION:=5e85d82b53f9c299caf378683294dfeedfdd029f
PKG_SOURCE:=rsReticulum-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/reticulum-spb/rsReticulum/tar.gz/$(PKG_SOURCE_VERSION)?
PKG_HASH:=f70c019daaf0108733d9b4fbf8c979edfbcef4dac0b40d4581946f48f47422ff

PKG_MAINTAINER:=RNS Gate
PKG_LICENSE:=AGPL-3.0-or-later
PKG_BUILD_DEPENDS:=$(if $(RNS_EXTERNAL_RUST),,rust/host)
PKG_BUILD_PARALLEL:=1
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)/rsReticulum
PKG_UNPACK=$(HOST_TAR) -C $(PKG_BUILD_DIR) --strip-components=1 -xzf $(DL_DIR)/$(PKG_SOURCE)

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/rust/rust-package.mk

CARGO_PKG_PROFILE:=release
CARGO_PKG_CONFIG_VARS += CARGO_TARGET_DIR=$(BUILD_DIR)/rns-cargo-target

define Package/rs-reticulum
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Reticulum implementation and command-line tools in Rust
  DEPENDS:=$(RUST_ARCH_DEPENDS)
  URL:=https://github.com/reticulum-spb/rsReticulum
endef

define Package/rs-reticulum/description
  Rust implementation of Reticulum and its command-line tools.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(SED) 's/as nix::libc::c_ulong;/as nix::libc::c_int;/' \
		$(PKG_BUILD_DIR)/crates/rns-runtime/src/rnsh.rs
endef

define Build/Compile
	$(call Build/Compile/Cargo,crates/rns-tools)
endef

define Package/rs-reticulum/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/etc/init.d $(1)/lib/upgrade/keep.d
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/* $(1)/usr/bin/
	$(INSTALL_BIN) ./files/rs-reticulum.init $(1)/etc/init.d/rs-reticulum
	$(INSTALL_DATA) ./files/rs-reticulum.keep $(1)/lib/upgrade/keep.d/rs-reticulum
endef

$(eval $(call RustBinPackage,rs-reticulum))
$(eval $(call BuildPackage,rs-reticulum))
