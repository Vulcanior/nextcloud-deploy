FROM caddy:2.7.6-alpine

COPY Caddyfile /etc/caddy/Caddyfile
RUN caddy fmt /etc/caddy/Caddyfile -w
RUN caddy validate --config /etc/caddy/Caddyfile