# Stage 1: compilación con Maven
FROM maven:3.9.9-eclipse-temurin-17-alpine AS builder
WORKDIR /app
COPY . .
RUN mvn clean package -DskipTests

# Stage 2: imagen final de ejecución
FROM eclipse-temurin:17-jre-alpine AS runner
RUN addgroup -S appgroup && adduser -S appuser -G appgroup
WORKDIR /app
COPY --from=builder /app/target/*.jar app.jar
RUN chown appuser:appgroup app.jar
USER appuser
EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
