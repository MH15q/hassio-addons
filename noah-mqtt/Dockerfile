FROM ghcr.io/home-assistant/base:latest

COPY config.json /
RUN apk --no-cache add jq curl tar

RUN NOAH_MQTT_VERSION=$(jq --raw-output '.version // empty' config.json) \
    && ARCH="$(uname -m)" \
    && case "$ARCH" in \
         aarch64) RELEASE_ARCH="arm64" ;; \
         armv7l|armv6l) RELEASE_ARCH="armv6" ;; \
         x86_64) RELEASE_ARCH="amd64" ;; \
         *) exit 1 ;; \
       esac \
    && curl -L -o /noah-mqtt.tar.gz "https://github.com/mtrossbach/noah-mqtt/releases/download/v${NOAH_MQTT_VERSION}/noah-mqtt_${NOAH_MQTT_VERSION}_linux_${RELEASE_ARCH}.tar.gz" \
    && tar -xf /noah-mqtt.tar.gz \
    && rm /noah-mqtt.tar.gz

COPY rootfs/docker-entrypoint.sh /
RUN chmod a+x /noah-mqtt /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]
