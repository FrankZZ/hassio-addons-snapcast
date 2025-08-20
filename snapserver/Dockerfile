ARG BUILD_FROM
FROM $BUILD_FROM

# Add env
ENV LANG C.UTF-8
ENV SNAPCAST_VERSION 0.32.1-r0

RUN \
    apk add --no-cache --virtual .build-dependencies \
    build-base \
    cargo \
    git \
    protobuf-dev \
    pulseaudio-dev \
    rust \
    && apk add --no-cache \
    pulseaudio \
    && cd /tmp \
    && git clone https://github.com/librespot-org/librespot.git \
    && cd librespot \
    && cargo build --release \
    --no-default-features \
    --features "with-libmdns pulseaudio-backend" \
    \
    && apk del --no-cache --purge .build-dependencies build-base cargo git protobuf-dev pulseaudio-dev \
    && cd \
    && rm -fr \
    /tmp/* \
    ~/.cargo \
    /usr/.crates.toml \
    /usr/.crates2.json

#Install snapcast
RUN sed -i -e 's/v[[:digit:]]\..*\//edge\//g' /etc/apk/repositories \
    # && apk add --no-cache librespot=${LIBRESPOT_VERSION} --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    && apk add --no-cache snapcast-server=${SNAPCAST_VERSION} bash

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
