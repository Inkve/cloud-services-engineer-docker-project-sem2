ARG NODE_VERSION=26
ARG APP_VERSION=1.0.0

# Build stage
FROM node:${NODE_VERSION}-alpine AS builder

WORKDIR /app

COPY package*.json ./

RUN npm ci --no-audit --no-fund && \
    npm cache clean --force

COPY . .

RUN NODE_OPTIONS=--openssl-legacy-provider npm run build

# Final stage (copy data)
FROM alpine:3.24.0

# Fix CVE-2026-45447
RUN apk add --no-cache \
    openssl=3.5.7-r0

ARG APP_VERSION
LABEL version="${APP_VERSION}"

WORKDIR /app

COPY --from=builder /app/dist /app/static