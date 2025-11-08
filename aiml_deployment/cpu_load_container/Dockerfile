# ==========================================
# Stage 1 — Builder
# ==========================================
FROM python:3.12-slim AS builder
WORKDIR /app

# Install system deps (for Pillow/OpenCV etc.)
RUN apt-get update && apt-get install -y libgl1 libglib2.0-0 && rm -rf /var/lib/apt/lists/*

# Copy requirements & install
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY app/ .

# ==========================================
# Stage 2 — Runtime
# ==========================================
FROM python:3.12-slim
WORKDIR /app

# Copy deps & code
COPY --from=builder /usr/local/lib/python3.12 /usr/local/lib/python3.12
COPY --from=builder /app /app

# Expose inference port
EXPOSE 8080

# Healthcheck
HEALTHCHECK CMD curl -f http://localhost:8080/docs || exit 1

# Non-root user
RUN adduser --disabled-password --gecos '' appuser
USER appuser

# Run FastAPI inference server
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8080"]
