# OpenWRT Feed for Reticulum packages

> [!WARNING]
> Work in progress. For now manual configuration of installed packages is required (e.g. no demons by default).<br>
> If you want to add an additional compilation [target](https://openwrt.org/docs/techref/targets/start) you can make PR or create an issue. 

## Feeds URLs

| Target                                            | Package architecture       | Feed index                                                                                           |
|---------------------------------------------------|----------------------------|------------------------------------------------------------------------------------------------------|
| `bcm27xx-bcm2710`<br>(Raspberry Pi 3b+, etc.)     | `aarch64_cortex-a53`       | `https://reticulum-spb.github.io/openwrt-rns-feed/25.12.5/aarch64_cortex-a53/rns/packages.adb`       |
| `rockchip-armv8`<br>(King3399, etc.)              | `aarch64_generic`          | `https://reticulum-spb.github.io/openwrt-rns-feed/25.12.5/aarch64_generic/rns/packages.adb`          |
| `sunxi-cortexa7`<br>(Orange Pi Zero H2+/H3, etc.) | `arm_cortex-a7_neon-vfpv4` | `https://reticulum-spb.github.io/openwrt-rns-feed/25.12.5/arm_cortex-a7_neon-vfpv4/rns/packages.adb` |

## Repository signing key

```sh
cat > /etc/apk/keys/rns.pem <<'EOF'
-----BEGIN PUBLIC KEY-----
MFkwEwYHKoZIzj0CAQYIKoZIzj0DAQcDQgAEPcvKiH4VESWWstHKLlZuJlJ0Yhik
WpM9kyAmO2Ir0LSxv08W+fSC0DZoz5zpzGuBi1ZM06qry1tjiOA+xiwTdA==
-----END PUBLIC KEY-----
EOF
```
