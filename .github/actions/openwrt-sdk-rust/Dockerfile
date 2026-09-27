ARG OPENWRT_SDK_IMAGE=ghcr.io/openwrt/sdk
ARG OPENWRT_SDK_TAG=mips_24kc
FROM ${OPENWRT_SDK_IMAGE}:${OPENWRT_SDK_TAG}

ARG RUST_TARGET
ARG RUST_VERSION=1.94.0

ENV RUSTUP_HOME=/builder/.rustup
ENV CARGO_HOME=/builder/.cargo
ENV PATH=/builder/.cargo/bin:${PATH}

RUN curl --proto '=https' --tlsv1.2 --fail --silent --show-error \
        https://sh.rustup.rs --output /tmp/rustup-init.sh \
    && sh /tmp/rustup-init.sh -y --no-modify-path --profile minimal \
        --default-toolchain "$RUST_VERSION" \
    && rustup target add "$RUST_TARGET" \
    && rm /tmp/rustup-init.sh

COPY --chmod=0755 entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
