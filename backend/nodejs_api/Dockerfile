# ==========================================
# Stage 1 — Builder
# ==========================================
FROM node:20-alpine AS builder

WORKDIR /app

# Copy package manifests first for caching
COPY package*.json ./

# Install dependencies
RUN npm ci

# Copy the source code
COPY . .

# Build (TypeScript -> JavaScript)
RUN npm run build

# ==========================================
# Stage 2 — Runtime
# ==========================================
FROM node:20-alpine AS runner

WORKDIR /app

# Copy only necessary files from builder
COPY package*.json ./
RUN npm ci --omit=dev

COPY --from=builder /app/dist ./dist

# Create non-root user for security
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

# Expose API port
EXPOSE 3000

# Healthcheck
HEALTHCHECK CMD wget -qO- http://localhost:3000/health || exit 1

# Start command
CMD ["node", "dist/main.js"]
