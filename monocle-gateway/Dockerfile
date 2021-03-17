ARG BUILD_FROM=ubuntu:bionic-20210118

FROM ${BUILD_FROM}


# ------------------------
# Environment variables
# ------------------------
ENV \
    DEBIAN_FRONTEND="noninteractive" \
    HOME="/root" \
    LANG="C.UTF-8" \
    PS1="$(whoami)@$(hostname):$(pwd)$ " \
    S6_BEHAVIOUR_IF_STAGE2_FAILS=2 \
    S6_CMD_WAIT_FOR_SERVICES=1 \
    TERM="xterm-256color"


# ------------------------
# Copy root filesystem
# ------------------------
COPY rootfs /


# ------------------------
# Set shell
# ------------------------
SHELL ["/bin/bash", "-o", "pipefail", "-c"]


# ------------------------
# Install base system,
# Monocle Gateway dependencies
# and other useful utilities
# ------------------------
ARG BUILD_ARCH=armv7
RUN \
    apt-get update \
    \
    && apt-get install -y --no-install-recommends \
        ca-certificates=20210119~18.04.1 \
        curl=7.58.0-2ubuntu3.12 \
        jq=1.5+dfsg-2 \
        tzdata=2021a-0ubuntu0.18.04 \
        wget \
        nano \
        net-tools \
        openssl \
    \
    && curl -L -s "https://github.com/just-containers/s6-overlay/releases/download/v2.2.0.3/s6-overlay-arm.tar.gz" \
        | tar zxvf - -C / \
    \
    && mkdir -p /etc/fix-attrs.d \
    && mkdir -p /etc/services.d \
    \
    && curl -J -L -o /tmp/bashio.tar.gz \
        "https://github.com/hassio-addons/bashio/archive/v0.13.0.tar.gz" \
    && mkdir /tmp/bashio \
    && tar zxvf \
        /tmp/bashio.tar.gz \
        --strip 1 -C /tmp/bashio \
    \
    && mv /tmp/bashio/lib /usr/lib/bashio \
    && ln -s /usr/lib/bashio/bashio /usr/bin/bashio \
    \
    && curl -L -s -o /usr/bin/tempio \
        "https://github.com/home-assistant/tempio/releases/download/2021.01.0/tempio_${BUILD_ARCH}" \
    && chmod a+x /usr/bin/tempio \
    \
    && rm -fr \
        /tmp/* \
        /var/{cache,log}/* \
        /var/lib/apt/lists/*




# ---------------------------------------
# Create Monocle Gateway configuration 
# directory
# ---------------------------------------
RUN mkdir -p /etc/monocle
RUN mkdir -p /tmp/monocle


# ---------------------------------------
# Download versioned Monocle Gateway
# build archive file
# - - - - - - - - - - - - - - - - - - - -
# Extract Moncole Gateway related 
# executables to the appropriate 
# runtime directories 
# - - - - - - - - - - - - - - - - - - - -
# Remove the downloaded Monocle Gateway 
# archive files
# ---------------------------------------
RUN wget -c https://files.monoclecam.com/monocle-gateway/linux/monocle-gateway-linux-armv7-v0.0.4.tar.gz -O /tmp/monocle/monocle-gateway.tar.gz && \
    cd /usr/local/bin/ && \
    tar xvzf /tmp/monocle/monocle-gateway.tar.gz monocle-gateway && \ 
    tar xvzf /tmp/monocle/monocle-gateway.tar.gz monocle-proxy  && \
    rm -Rf /tmp/monocle/



# ---------------------------------------
# Entrypoint
# ---------------------------------------
ENTRYPOINT [ "/init" ]


# ---------------------------------------
# Expose required TCP ports 
# (port 443 is required by Amazon for 
# secure connectivity)
# ---------------------------------------
EXPOSE 443/tcp

# ---------------------------------------
# Expose required UDP ports 
# (used for the @proxy method to allow 
# IP cameras to transmit streams via UDP)
# ---------------------------------------
EXPOSE 62000-62100/udp

# ---------------------------------------
# Copy data for add-on
# ---------------------------------------
COPY run.sh /
RUN chmod a+x /run.sh

CMD ["/run.sh"]


# ---------------------------------------
# Build arugments
# ---------------------------------------
ARG BUILD_DATE
ARG BUILD_REF
ARG BUILD_VERSION
ARG BUILD_REPOSITORY


# ---------------------------------------
# Labels
# ---------------------------------------
LABEL \
    io.hass.name="HA Monocle Gateway for ${BUILD_ARCH}" \
    io.hass.description="Home Assistant Monocle Gateway for ${BUILD_ARCH}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    io.hass.base.version=${BUILD_VERSION} \
    maintainer="GKaras <gkaras@xirografiki.gr>" \
    org.opencontainers.image.title="Addon Monocle Gateway for ${BUILD_ARCH}" \
    org.opencontainers.image.description="Monocle Gateway for Home Assistant" \
    org.opencontainers.image.vendor="" \
    org.opencontainers.image.authors="GKaras <gkaras@xirografiki.gr>" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}