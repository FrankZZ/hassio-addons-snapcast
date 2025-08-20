ARG BUILD_FROM
FROM $BUILD_FROM

# Add env
ENV LANG=C.UTF-8
ENV SNAPCAST_VERSION=0.32.1-r0

RUN \
    apk add --no-cache \
    pulseaudio \
    && curl -o /usr/bin/librespot https://github.com/music-assistant/server/raw/refs/heads/dev/music_assistant/providers/spotify/bin/librespot-linux-x86_64 \
    && chmod a+x /usr/bin/librespot

#Install snapcast
RUN sed -i -e 's/v[[:digit:]]\..*\//edge\//g' /etc/apk/repositories \
    # && apk add --no-cache librespot=${LIBRESPOT_VERSION} --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing \
    && apk add --no-cache snapcast-server=${SNAPCAST_VERSION} bash

# Copy data for add-on
COPY run.sh /
RUN chmod a+x /run.sh

CMD [ "/run.sh" ]
