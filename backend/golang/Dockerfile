# ==========================================
# Stage 1 — Builder
# ==========================================
FROM golang:1.23-alpine AS builder

# Set working directory
WORKDIR /app

# Copy go mod files and download dependencies
COPY go.mod go.sum ./
RUN go mod download

# Copy source code
COPY . .

# Build static binary
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o server ./cmd/main.go

# ==========================================
# Stage 2 — Runtime
# ==========================================
FROM gcr.io/distroless/base-debian12

# Set working directory
WORKDIR /app

# Copy compiled binary from builder
COPY --from=builder /app/server .

# Create non-root user (distroless is already non-root)
USER nonroot:nonroot

# Expose port
EXPOSE 8080

# Healthcheck (optional)
# HEALTHCHECK CMD wget -qO- http://localhost:8080/health || exit 1

# Run binary
ENTRYPOINT ["./server"]
