FROM --platform=$TARGETOS/$TARGETARCH node:23-bookworm-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# --------------------------------------------------
# System + container user
# --------------------------------------------------
RUN useradd -m -d /home/container container
STOPSIGNAL SIGINT

RUN apt update && apt -y install \
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
    tini \
 && rm -rf /var/lib/apt/lists/*

# --------------------------------------------------
# Node tooling
# --------------------------------------------------
RUN npm install --global npm@latest typescript ts-node @types/node

RUN npm install -g corepack \
 && corepack enable \
 && corepack prepare pnpm@latest --activate

# --------------------------------------------------
# Install Deno system-wide (yt-dlp JS solver)
# --------------------------------------------------
RUN curl -fsSL https://deno.land/install.sh | sh \
 && mv /root/.deno/bin/deno /usr/local/bin/deno \
 && chmod +x /usr/local/bin/deno \
 && deno --version

# --------------------------------------------------
# Switch to container user
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
