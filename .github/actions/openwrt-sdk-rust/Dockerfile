ARG CONTAINER=ghcr.io/openwrt/sdk
ARG ARCH=mips_24kc
ARG RUST_TARGET
FROM $CONTAINER:$ARCH

ARG RUST_TARGET

ENV RUSTUP_HOME=/builder/.rustup
ENV CARGO_HOME=/builder/.cargo
ENV PATH=/builder/.cargo/bin:${PATH}

RUN curl --proto '=https' --tlsv1.2 --fail --silent --show-error \
        https://sh.rustup.rs --output /tmp/rustup-init.sh \
    && sh /tmp/rustup-init.sh -y --no-modify-path --profile minimal \
        --default-toolchain 1.94.0 \
    && rustup target add "$RUST_TARGET" \
    && rm /tmp/rustup-init.sh

COPY --chmod=0755 entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
