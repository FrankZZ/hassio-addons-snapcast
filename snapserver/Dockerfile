ARG BUILD_FROM
FROM $BUILD_FROM

# Add env
ENV LANG C.UTF-8
ENV SNAPCAST_VERSION=0.32.2
ENV LIBRESPOT_COMMIT=78ce118d32912adfb2705481f69c83df6a88211f

ENV PATH="/root/.cargo/bin:${PATH}"
RUN \
    apt-get update \
    && apt-get -y install build-essential git \
    && cd /tmp \
    && curl -LO "https://github.com/badaix/snapcast/releases/download/v${SNAPCAST_VERSION}/snapserver_${SNAPCAST_VERSION}-1_amd64_bookworm.deb" \
    && apt-get install -y ./snapserver_${SNAPCAST_VERSION}-1_amd64_bookworm.deb \
    && rm snapserver_${SNAPCAST_VERSION}-1_amd64_bookworm.deb \
    && mkdir librespot \
    && cd librespot \
    && git init \
    && git remote add origin https://github.com/librespot-org/librespot.git \
    && git fetch --depth=1 origin "${LIBRESPOT_COMMIT}" \
    && git checkout FETCH_HEAD \
    && curl https://sh.rustup.rs -sSf | sh -s -- -y \
    # && apt-get install -y libasound2-dev \
    && cargo build --release \
    --no-default-features \
    --features "with-libmdns rustls-tls-native-roots" \
    && apt-get purge -y build-essential git \
    && apt-get clean \
    && cd \
    && rm -fr \
    /tmp/* \
    ~/.cargo \
    /usr/.crates.toml \
    /usr/.crates2.json

#Install snapcast
# RUN sed -i -e 's/v[[:digit:]]\..*\//edge\//g' /etc/apk/repositories \
#     # && apk add --no-cache librespot=${LIBRESPOT_VERSION} --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing \
#     && apk add --no-cache snapcast-server=${SNAPCAST_VERSION} bash --repository=http://dl-cdn.alpinelinux.org/alpine/edge/main

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
