FROM nginx:1.31.1-alpine-slim

RUN apk add --no-cache \
    openssl=3.5.7-r0

RUN addgroup -S nginx_user && \
    adduser -S -s /sbin/nologin nginx_user -G nginx_user

COPY nginx.conf /etc/nginx/conf.d/default.conf

RUN chown -R nginx_user:nginx_user /var/cache/nginx && \
    chown -R nginx_user:nginx_user /var/log/nginx && \
    chown -R nginx_user:nginx_user /etc/nginx/conf.d && \
    touch /var/run/nginx.pid && \
    chown -R nginx_user:nginx_user /var/run/nginx.pid

USER nginx_user

EXPOSE 80