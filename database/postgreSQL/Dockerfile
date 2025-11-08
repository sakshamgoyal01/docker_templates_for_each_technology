FROM postgres:16-alpine

# Copy init scripts
COPY init.sql /docker-entrypoint-initdb.d/

# Environment variables
ENV POSTGRES_DB=appdb \
  POSTGRES_USER=appuser \
  POSTGRES_PASSWORD=secret123

EXPOSE 5432
