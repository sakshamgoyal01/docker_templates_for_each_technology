FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# ============================
# Stage 2 — Runtime Stage
# ============================
FROM node:20-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production

# Copy only required build output and deps
COPY --from=builder /app/package*.json ./
RUN npm ci --omit=dev
COPY --from=builder /app/.next ./.next
COPY --from=builder /app/public ./public

EXPOSE 3000
HEALTHCHECK CMD wget -qO- http://localhost:3000/ || exit 1
CMD ["npm", "start"]