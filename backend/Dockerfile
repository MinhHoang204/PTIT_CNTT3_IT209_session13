# Stage 1: Build ứng dụng bằng Gradle
FROM gradle:8.10.2-jdk21 AS build

WORKDIR /app

# Sao chép cấu hình Gradle trước để tận dụng cache
COPY gradlew .
COPY gradle ./gradle
COPY build.gradle* settings.gradle* ./

RUN chmod +x gradlew

# Sao chép source code
COPY src ./src

# Build Spring Boot executable JAR
RUN ./gradlew --no-daemon bootJar -x test \
    && JAR_FILE=$(find build/libs -maxdepth 1 -type f -name "*.jar" ! -name "*-plain.jar" -print -quit) \
    && test -n "$JAR_FILE" \
    && cp "$JAR_FILE" /app/app.jar

# Stage 2: Chạy ứng dụng
FROM eclipse-temurin:21-jre

WORKDIR /app

COPY --from=build /app/app.jar app.jar

EXPOSE 8080

ENTRYPOINT ["java", "-jar", "app.jar"]