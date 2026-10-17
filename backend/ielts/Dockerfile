# ==============================================================================
# Multi-stage Dockerfile: Spring Boot 3 + Java 17 + Praat CLI + CMU Pronouncing Dict
# Optimized for zero-cost deployment (Render.com, Railway, Fly.io)
# ==============================================================================

# --- Stage 1: Build JAR ---
FROM maven:3.9.6-eclipse-temurin-17 AS builder
WORKDIR /app

# Cache Maven dependencies
COPY pom.xml .
RUN mvn dependency:go-offline -B

# Build application without running tests for fast compilation
COPY src ./src
RUN mvn clean package -DskipTests -B

# --- Stage 2: Runtime Image (Debian-based for apt package compatibility) ---
FROM eclipse-temurin:17-jre
WORKDIR /app

# Install Praat phonetics CLI, ffmpeg (audio conversions), and curl (to fetch CMU Dict)
RUN apt-get update && apt-get install -y --no-install-recommends \
    praat \
    ffmpeg \
    curl \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# Download CMU Pronouncing Dictionary directly into /app/cmudict-0.7b.txt
RUN curl -fsSL -o /app/cmudict-0.7b.txt \
    https://raw.githubusercontent.com/Alexir/CMUdict/master/cmudict-0.7b

# Copy Praat scripts from backend source to /app/praat/
RUN mkdir -p /app/praat
COPY --from=builder /app/src/main/java/web/ielts/Test/*.praat /app/praat/

# Create a dedicated non-root application user
RUN groupadd -r spring && useradd -r -g spring spring \
    && chown -R spring:spring /app

USER spring:spring

# Copy built JAR artifact
COPY --from=builder --chown=spring:spring /app/target/*.jar app.jar

# Expose Spring Boot port
EXPOSE 8080

# Environment defaults
ENV PORT=8080
ENV JAVA_OPTS="-XX:+UseContainerSupport -XX:MaxRAMPercentage=75.0"
ENV PRAAT_PATH="/usr/bin/praat"
ENV PRAAT_SCRIPT_PATH="/app/praat/script.praat"
ENV PRAAT_STRESS_SCRIPT_PATH="/app/praat/stressAnalysis.praat"
ENV PRAAT_INTONATION_SCRIPT_PATH="/app/praat/script_intonation.praat"
ENV PRAAT_DURATION_SCRIPT_PATH="/app/praat/getDuration.praat"
ENV CMU_DICT_PATH="/app/cmudict-0.7b.txt"

ENTRYPOINT ["sh", "-c", "java $JAVA_OPTS -jar app.jar --server.port=${PORT}"]
