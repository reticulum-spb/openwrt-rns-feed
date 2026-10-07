include $(TOPDIR)/rules.mk

PKG_NAME:=reticulum
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_MAINTAINER:=RNS Gate
PKG_LICENSE:=AGPL-3.0-or-later

include $(INCLUDE_DIR)/package.mk

define Package/reticulum
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Reticulum Network Stack installed from PyPI
  PKGARCH:=all
  DEPENDS:=+python3 +python3-pip +python3-cryptography +ca-bundle
  URL:=https://reticulum.network/
endef

define Package/reticulum/description
  Installs the Reticulum Network Stack from PyPI using pip.
  Internet access is required during installation.
endef

define Package/reticulum/conffiles
/root/.reticulum/config
/root/.rnsh/allowed_identities
endef

define Build/Configure
endef

define Build/Compile
endef

define Package/reticulum/install
	$(INSTALL_DIR) $(1)/root/.reticulum $(1)/root/.rnsh $(1)/etc/init.d
	$(INSTALL_CONF) ./files/rns-config $(1)/root/.reticulum/config
	$(INSTALL_CONF) ./files/rnsh-allowed-identities $(1)/root/.rnsh/allowed_identities
	$(INSTALL_BIN) ./files/reticulum.init $(1)/etc/init.d/reticulum
	$(INSTALL_BIN) ./files/rnsh.init $(1)/etc/init.d/rnsh
endef

define Package/reticulum/postinst
#!/bin/sh
if [ "$$PKG_UPGRADE" != "1" ]; then
	for service in reticulum rnsh; do
		sh "$$IPKG_INSTROOT/etc/rc.common" "$$IPKG_INSTROOT/etc/init.d/$$service" disable
	done
fi
[ -n "$$IPKG_INSTROOT" ] && exit 0
python3 -m pip --disable-pip-version-check install --no-cache-dir --index-url https://pypi.org/simple rns
endef

define Package/reticulum/prerm
#!/bin/sh
[ -n "$$IPKG_INSTROOT" ] && exit 0
[ "$$PKG_UPGRADE" = "1" ] && exit 0
for service in reticulum rnsh; do
	[ -x "/etc/init.d/$$service" ] || continue
	"/etc/init.d/$$service" stop
	"/etc/init.d/$$service" disable
done
exit 0
endef

$(eval $(call BuildPackage,reticulum))
