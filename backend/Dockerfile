# Build stage
FROM golang:1.24 AS builder
WORKDIR /app

COPY go.mod go.sum ./
RUN go mod download && go mod verify

COPY . .
RUN CGO_ENABLED=0 GOOS=linux go build -o server-app ./cmd/api

# Runtime stage
FROM alpine:3.22
WORKDIR /app

COPY --from=builder /app/server-app .

EXPOSE 8081
CMD ["./server-app"]