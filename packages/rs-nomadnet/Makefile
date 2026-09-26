include $(TOPDIR)/rules.mk

PKG_NAME:=rs-nomadnet
PKG_VERSION:=0.1.0
PKG_RELEASE:=1

PKG_SOURCE_VERSION:=8639fabf243e57623bed4a9d6ba40def043da245
PKG_SOURCE:=rsNomadNet-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/reticulum-spb/rsNomadNet/tar.gz/$(PKG_SOURCE_VERSION)?
PKG_HASH:=ee73c359473bce74e4e7b72b0e3dea1e508dd12c9753f9714dadb4d6ac96a535

RSRETICULUM_VERSION:=5e85d82b53f9c299caf378683294dfeedfdd029f
RSRETICULUM_SOURCE:=rsReticulum-$(RSRETICULUM_VERSION).tar.gz
RSLXMF_VERSION:=7383ab6a31f3af74fb8f3fd74948795b78d6c473
RSLXMF_SOURCE:=rsLXMF-$(RSLXMF_VERSION).tar.gz
RSRRC_VERSION:=6307592f77ddc5f099c8d7f5a35824d8326de9ff
RSRRC_SOURCE:=rsRRC-$(RSRRC_VERSION).tar.gz
RSRRC_CLIENT_VERSION:=c31425dd3a5fd5aba66528322cfaf84d4fa8665d
RSRRC_CLIENT_SOURCE:=rsRRC-client-$(RSRRC_CLIENT_VERSION).tar.gz

PKG_MAINTAINER:=RNS Gate
PKG_LICENSE:=AGPL-3.0-or-later
PKG_BUILD_DEPENDS:=$(if $(RNS_EXTERNAL_RUST),,rust/host)
PKG_BUILD_PARALLEL:=1
PKG_BUNDLE_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_VERSION)
PKG_BUILD_DIR:=$(PKG_BUNDLE_DIR)/rsNomadNet
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

define Download/rslxmf
  URL:=https://codeload.github.com/reticulum-spb/rsLXMF/tar.gz/$(RSLXMF_VERSION)?
  FILE:=$(RSLXMF_SOURCE)
  HASH:=74499347357829a01ae52271a38714b05872d522f4dea153023fd5e0cfc5a733
endef

define Download/rsrrc
  URL:=https://codeload.github.com/reticulum-spb/rsRRC/tar.gz/$(RSRRC_VERSION)?
  FILE:=$(RSRRC_SOURCE)
  HASH:=541c7a5079ed4e4bd57f07d45871381add9cdbf26606a710ad3dec09c0a6b6e6
endef

define Download/rsrrc-client
  URL:=https://codeload.github.com/reticulum-spb/rsRRC-client/tar.gz/$(RSRRC_CLIENT_VERSION)?
  FILE:=$(RSRRC_CLIENT_SOURCE)
  HASH:=600688f137a4e986a8c81c890974beff2b29e87ee36401bdeb71668128cebdeb
endef

define Package/rs-nomadnet
  SECTION:=net
  CATEGORY:=Network
  TITLE:=NomadNet implementation in Rust
  DEPENDS:=$(RUST_ARCH_DEPENDS)
  URL:=https://github.com/reticulum-spb/rsNomadNet
endef

define Package/rs-nomadnet/description
  Rust command-line implementation of NomadNet.
endef

define Build/Prepare
	$(call Build/Prepare/Default)
	$(eval $(call Download,rsreticulum))
	$(eval $(call Download,rslxmf))
	$(eval $(call Download,rsrrc))
	$(eval $(call Download,rsrrc-client))
	rm -rf $(PKG_BUNDLE_DIR)/rsReticulum $(PKG_BUNDLE_DIR)/rsLXMF \
		$(PKG_BUNDLE_DIR)/rsRRC $(PKG_BUNDLE_DIR)/rsRRC-client
	mkdir -p $(PKG_BUNDLE_DIR)/rsReticulum $(PKG_BUNDLE_DIR)/rsLXMF \
		$(PKG_BUNDLE_DIR)/rsRRC $(PKG_BUNDLE_DIR)/rsRRC-client
	$(TAR) -xf $(DL_DIR)/$(RSRETICULUM_SOURCE) --strip=1 -C $(PKG_BUNDLE_DIR)/rsReticulum
	$(TAR) -xf $(DL_DIR)/$(RSLXMF_SOURCE) --strip=1 -C $(PKG_BUNDLE_DIR)/rsLXMF
	$(TAR) -xf $(DL_DIR)/$(RSRRC_SOURCE) --strip=1 -C $(PKG_BUNDLE_DIR)/rsRRC
	$(TAR) -xf $(DL_DIR)/$(RSRRC_CLIENT_SOURCE) --strip=1 -C $(PKG_BUNDLE_DIR)/rsRRC-client
	$(SED) 's/as nix::libc::c_ulong;/as nix::libc::c_int;/' \
		$(PKG_BUNDLE_DIR)/rsReticulum/crates/rns-runtime/src/rnsh.rs
endef

$(eval $(call RustBinPackage,rs-nomadnet))
$(eval $(call BuildPackage,rs-nomadnet))
