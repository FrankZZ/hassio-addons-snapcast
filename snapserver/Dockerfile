ARG BUILD_FROM
FROM $BUILD_FROM

# Add env
ENV LANG C.UTF-8
ENV SNAPCAST_VERSION 0.31.0-r0
ENV LIBRESPOT_VERSION 0.6.0-r0

# RUN \
#     apk add --no-cache --virtual .build-dependencies \
#     build-base \
#     cargo \
#     git \
#     protobuf-dev \
#     pulseaudio-dev \
#     \
#     && apk add --no-cache \
#     pulseaudio \
#     \
#     && cargo install \
#     --no-default-features \
#     --features pulseaudio-backend \
#     --root /usr \
#     --bin librespot \
#     --version ${LIBRESPOT_VERSION} \
#     --verbose \ 
#     -- librespot \
#     \
#     && apk del --no-cache --purge .build-dependencies \
#     && rm -fr \
#     /tmp/* \
#     ~/.cargo \
#     /usr/.crates.toml \
#     /usr/.crates2.json

#Install snapcast
RUN sed -i -e 's/v[[:digit:]]\..*\//edge\//g' /etc/apk/repositories \
    && apk add --no-cache librespot=${LIBRESPOT_VERSION} --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    && apk add --no-cache snapcast-server=${SNAPCAST_VERSION} bash

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
