# Stage 1: Build
FROM golang:1.24-alpine AS builder

WORKDIR /app

RUN addgroup -g 1001 appgroup && \
    adduser -u 1001 -G appgroup -s /bin/sh -D appuser

COPY go.mod go.sum ./
RUN go mod download

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o server ./cmd/server/main.go

# Stage 2: Runtime
FROM scratch

WORKDIR /app

COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

COPY --from=builder /app/server .

COPY --from=builder /app/migrations ./migrations

COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /etc/group /etc/group

USER appuser

EXPOSE 8080

CMD ["./server"]