# ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base-debian:trixie
ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.22
# FROM rust:1.88-alpine3.22 AS builder
# # Add env
# ENV LANG=C.UTF-8
# ENV LIBRESPOT_COMMIT=987dfa5df2546a96d34582674a757a3dcc6036a7
# ENV PATH="/root/.cargo/bin:${PATH}"
# # --repository=http://dl-cdn.alpinelinux.org/alpine/edge/main \
# RUN \
#     apk add --no-cache --virtual .build-dependencies \
#     build-base \
#     git \
#     libc-dev \
#     musl-dev \
#     protobuf-dev \
#     && cd /tmp \
#     # && curl https://sh.rustup.rs -sSf | sh -s -- -y \
#     && mkdir librespot \
#     && cd librespot \
#     && git init \
#     && git remote add origin https://github.com/librespot-org/librespot.git \
#     && git fetch --depth=1 origin ${LIBRESPOT_COMMIT} \
#     && git checkout FETCH_HEAD \
#     && cargo build -j4 --release \
#     --no-default-features \
#     --features "with-libmdns rustls-tls-native-roots" \
#     && cp target/release/librespot /librespot \
#     && apk del --no-cache --purge .build-dependencies build-base git protobuf-dev musl-dev libc-dev  \
#     && cd \
#     && rm -fr \
#     /tmp/* \
#     ~/.cargo \
#     /usr/.crates.toml \
#     /usr/.crates2.json

# Copy data for add-on
FROM $BUILD_FROM
ENV SNAPCAST_VERSION=0.34.0-r0
ENV LIBRESPOT_VERSION=0.7.1-r0
ENV PATH="/root/.cargo/bin:${PATH}"
RUN apk add --no-cache snapcast-server=${SNAPCAST_VERSION} --repository=https://dl-cdn.alpinelinux.org/alpine/edge/community
RUN apk add --no-cache librespot=${LIBRESPOT_VERSION} --repository=https://dl-cdn.alpinelinux.org/alpine/edge/testing
# RUN \
#     cd /tmp \
#     && apt-get update \
#     && curl -LO "https://github.com/badaix/snapcast/releases/download/v${SNAPCAST_VERSION}/snapserver_${SNAPCAST_VERSION}-1_amd64_bookworm.deb" \
#     && curl -LO "http://ftp.nl.debian.org/debian/pool/main/f/flac/libflac12_1.4.2+ds-2_amd64.deb" \
#     && apt-get install -y ./libflac12_1.4.2+ds-2_amd64.deb ./snapserver_${SNAPCAST_VERSION}-1_amd64_bookworm.deb \
#     && apt-get clean \
#     && rm snapserver_${SNAPCAST_VERSION}-1_amd64_bookworm.deb \
#     && rm libflac12_1.4.2+ds-2_amd64.deb

# COPY --from=builder /librespot /librespot
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
