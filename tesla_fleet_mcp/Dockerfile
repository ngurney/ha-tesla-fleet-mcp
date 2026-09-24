ARG BUILD_FROM=ghcr.io/home-assistant/aarch64-base-debian:bookworm
FROM ${BUILD_FROM}

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update \
  && apt-get install -y --no-install-recommends curl ca-certificates gnupg \
  && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
  && apt-get install -y --no-install-recommends nodejs \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

WORKDIR /opt/tesla-fleet-mcp
COPY app/package.json app/package-lock.json ./
RUN npm ci --omit=dev \
  && npm install tsx@4.20.0 --no-save

COPY app/ /opt/tesla-fleet-mcp/

COPY rootfs /
RUN chmod a+x /etc/services.d/tesla_fleet_mcp/run \
  && chmod a+x /etc/services.d/tesla_fleet_mcp/finish

