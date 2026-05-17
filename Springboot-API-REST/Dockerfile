# Stage 1: compilación con Maven
# eclipse-temurin incluye Maven y JDK 17. Compila el proyecto
# y genera el JAR en target/. La imagen completa pesa ~500MB
# pero el stage final NO la incluye.
FROM eclipse-temurin:17-jdk-alpine AS builder
WORKDIR /app
COPY . .
RUN chmod +x mvnw
RUN ./mvnw clean package -DskipTests

# Stage 2: imagen final de ejecución
# Solo JRE (no JDK), sin Maven, sin código fuente.
# Resultado: imagen ~200MB en lugar de ~500MB.
FROM eclipse-temurin:17-jre-alpine AS runner

# Principio de mínimo privilegio: usuario sin permisos root.
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app

# Solo copiamos el JAR compilado del stage anterior.
COPY --from=builder /app/target/*.jar app.jar

RUN chown appuser:appgroup app.jar

USER appuser

EXPOSE 8080
CMD ["java", "-jar", "app.jar"]
