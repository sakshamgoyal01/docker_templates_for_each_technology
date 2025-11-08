# ==========================================
# Stage 1 — Builder
# ==========================================
FROM python:3.12-slim AS builder
WORKDIR /app

# Install dependencies
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application
COPY app/ .

# ==========================================
# Stage 2 — Runtime
# ==========================================
FROM python:3.12-slim
WORKDIR /app

# Copy dependencies from builder
COPY --from=builder /usr/local/lib/python3.12 /usr/local/lib/python3.12
COPY --from=builder /app /app

# Non-root user
RUN adduser --disabled-password --gecos '' appuser
USER appuser

# Healthcheck (optional: ping Redis or self-test)
HEALTHCHECK CMD python -c "import redis; redis.Redis('redis',6379).ping()" || exit 1

# Start Celery worker
CMD ["celery", "-A", "tasks", "worker", "--loglevel=info"]
