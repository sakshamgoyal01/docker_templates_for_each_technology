# ==========================================
# Stage 1 — Builder
# ==========================================
FROM python:3.12-slim AS builder

WORKDIR /app

# Install dependencies separately for layer caching
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy entire application
COPY app/ .

# ==========================================
# Stage 2 — Runtime
# ==========================================
FROM python:3.12-slim

# Set working directory
WORKDIR /app

# Copy installed dependencies from builder
COPY --from=builder /usr/local/lib/python3.12 /usr/local/lib/python3.12

# Copy app source code
COPY --from=builder /app /app

# Create a non-root user for security
RUN adduser --disabled-password --gecos '' appuser
USER appuser

# Expose the app port
EXPOSE 8080

# Healthcheck for Kubernetes
HEALTHCHECK CMD curl -f http://localhost:8080/health || exit 1

# Start FastAPI using Uvicorn
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8080"]
