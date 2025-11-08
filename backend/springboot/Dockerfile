# ==========================================
# Stage 1 — Build Stage
# ==========================================
FROM maven:3.9.9-eclipse-temurin-21 AS builder

# Set working directory
WORKDIR /app

# Copy pom.xml and download dependencies (for caching)
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Copy source code
COPY src ./src

# Build the application (skip tests for faster CI build)
RUN mvn clean package -DskipTests

# ==========================================
# Stage 2 — Runtime Stage
# ==========================================
FROM eclipse-temurin:21-jre-jammy

# Set environment variables
ENV APP_HOME=/opt/app \
  JAVA_OPTS="-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0 -XX:+ExitOnOutOfMemoryError"

WORKDIR $APP_HOME

# Copy built jar from builder stage
COPY --from=builder /app/target/*.jar app.jar

# Create non-root user
RUN groupadd -r appgroup && useradd -r -g appgroup appuser
USER appuser

# Expose Spring Boot default port
EXPOSE 8080

# Health check for Kubernetes
HEALTHCHECK --interval=30s --timeout=5s --start-period=30s \
  CMD curl -f http://localhost:8080/actuator/health || exit 1

# Start the app
ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar"]
