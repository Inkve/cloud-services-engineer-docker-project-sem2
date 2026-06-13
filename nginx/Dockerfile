FROM nginx:stable-alpine3.21-slim

RUN rm /etc/nginx/conf.d/default.conf

COPY nginx.conf /etc/nginx/templates/default.conf.template

EXPOSE 80