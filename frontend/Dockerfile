# Build stage
FROM node:26-alpine AS builder

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

WORKDIR /app

COPY --from=builder /app/dist /app/static