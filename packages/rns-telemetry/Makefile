include $(TOPDIR)/rules.mk

PKG_NAME:=rns-telemetry
PKG_VERSION:=1.0.0
PKG_RELEASE:=1

PKG_MAINTAINER:=RNS Gate
PKG_LICENSE:=AGPL-3.0-or-later

include $(INCLUDE_DIR)/package.mk

define Package/rns-telemetry
  SECTION:=net
  CATEGORY:=Network
  TITLE:=RNS Gate logging and metrics dependencies
  PKGARCH:=all
  DEPENDS:=+syslog-ng +fluent-bit +ca-bundle
endef

define Package/rns-telemetry/description
  Installs syslog-ng and Fluent Bit with system metrics export over OTLP HTTP.
endef

define Package/rns-telemetry/conffiles
/etc/syslog-ng.d/syslog-ng-rns.conf
/etc/fluent-bit/fluent-bit.yaml
endef

define Build/Compile
endef

define Package/rns-telemetry/install
	$(INSTALL_DIR) $(1)/etc/syslog-ng.d $(1)/etc/syslog-ng $(1)/etc/fluent-bit $(1)/etc/uci-defaults
	$(INSTALL_CONF) ./files/syslog-ng.conf $(1)/etc/syslog-ng.d/syslog-ng-rns.conf
	$(INSTALL_CONF) ./files/fluent-bit.yaml $(1)/etc/fluent-bit/fluent-bit.yaml
	$(INSTALL_BIN) ./files/rns-telemetry.defaults $(1)/etc/uci-defaults/rns-telemetry
endef

define Package/rns-telemetry/postinst
#!/bin/sh
[ -n "$$IPKG_INSTROOT" ] || {
	[ -s /etc/fluent-bit/device-mac ] || exit 1
	[ -s /etc/syslog-ng/device-mac.conf ] || exit 1
	/etc/init.d/syslog-ng reload
	if /etc/init.d/fluent-bit running; then
		/etc/init.d/fluent-bit restart
	else
		/etc/init.d/fluent-bit start
	fi
}
endef

define Package/rns-telemetry/prerm
#!/bin/sh
[ -n "$$IPKG_INSTROOT" ] || {
	if /etc/init.d/fluent-bit running; then
		/etc/init.d/fluent-bit stop
	fi
}
exit 0
endef

define Package/rns-telemetry/postrm
#!/bin/sh
rm -f "$$IPKG_INSTROOT/etc/syslog-ng.d/syslog-ng-rns.conf" \
	"$$IPKG_INSTROOT/etc/syslog-ng/device-mac.conf" \
	"$$IPKG_INSTROOT/etc/fluent-bit/device-mac"
[ -n "$$IPKG_INSTROOT" ] || {
	[ ! -x /etc/init.d/syslog-ng ] || /etc/init.d/syslog-ng reload
}
exit 0
endef

$(eval $(call BuildPackage,rns-telemetry))
