FROM --platform=$TARGETOS/$TARGETARCH node:14-bullseye-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# --------------------------------------------------
# System + container user
# --------------------------------------------------
RUN useradd -m -d /home/container container
STOPSIGNAL SIGINT

# --------------------------------------------------
# System dependencies (hardened APT)
# --------------------------------------------------
RUN set -eux; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        ffmpeg \
        iproute2 \
        git \
        sqlite3 \
        libsqlite3-dev \
        python3 \
        python3-dev \
        ca-certificates \
        dnsutils \
        tzdata \
        zip \
        tar \
        curl \
        cmake \
        build-essential \
        libtool \
        iputils-ping \
        libnss3 \
        tini; \
    rm -rf /var/lib/apt/lists/*

# --------------------------------------------------
# Node tooling
# --------------------------------------------------
RUN npm install -g \
    npm@latest \
    typescript \
    ts-node \
    @types/node

# --------------------------------------------------
# Runtime user
# --------------------------------------------------
USER container
ENV USER=container HOME=/home/container
WORKDIR /home/container

# --------------------------------------------------
# Entrypoint
# --------------------------------------------------
COPY --chown=container:container ./../entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]
