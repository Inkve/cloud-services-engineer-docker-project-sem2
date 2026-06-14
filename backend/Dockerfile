ARG APP_VERSION=1.0.0
ARG GO_VERSION=1.26

# Build stage
FROM golang:${GO_VERSION}-alpine AS builder
WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .
RUN CGO_ENABLED=0 GOOS=linux \
    go build -trimpath -ldflags="-s -w -X main.version=${APP_VERSION}" \
    -o server-app ./cmd/api

# Runtime stage
FROM alpine:3.24.0

# Fix CVE-2026-45447
RUN apk add --no-cache \
    openssl=3.5.7-r0

ARG APP_VERSION
LABEL version="${APP_VERSION}"

WORKDIR /app

RUN addgroup -S app_user && \
    adduser -S -s /sbin/nologin -G app_user app_user

COPY --from=builder --chown=app_user:app_user /app/server-app .

USER app_user

EXPOSE 8081

HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD wget -q -O /dev/null http://127.0.0.1:8081/health || exit 1

CMD ["./server-app"]