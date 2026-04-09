FROM prom/prometheus:v3.11.0 AS prometheus

FROM alpine:3.21

RUN apk add --no-cache gettext

COPY --from=prometheus /bin/prometheus /bin/prometheus

RUN mkdir -p /etc/prometheus /prometheus \
    && chown -R nobody:nogroup /etc/prometheus /prometheus

WORKDIR /prometheus