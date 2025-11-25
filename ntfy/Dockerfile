FROM binwiederhier/ntfy:v2.15.0 AS base

ARG BUILD_VERSION
ARG BUILD_ARCH

LABEL \
    io.hass.version="${BUILD_VERSION}" \
    io.hass.type="addon" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.name="ntfy" \
    org.opencontainers.image.title="ntfy Home Assistant Add-on"

USER root
RUN apk add --no-cache --update \
    jq \
    bash

WORKDIR /data

COPY run.sh /app/run.sh
RUN chmod +x /app/run.sh

# Expose port
EXPOSE 8080

# Health check
HEALTHCHECK --interval=30s --timeout=10s --start-period=30s --retries=3 \
    CMD wget --no-verbose --tries=1 --spider http://localhost:8080/v1/health || exit 1

ENTRYPOINT ["/app/run.sh"]

# Final stage for Home Assistant
FROM base AS final