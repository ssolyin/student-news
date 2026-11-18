# ---- Stage 1: build the JAR (JDK + Maven) ----
FROM maven:3.10-eclipse-temurin-27 AS build
WORKDIR /app
COPY pom.xml .
RUN mvn -B -q dependency:go-offline
COPY src ./src
RUN mvn -B -q clean package -DskipTests

# ---- Stage 2: run it (JRE only, Alpine) ----
FROM eclipse-temurin:27-jre-alpine
WORKDIR /app
RUN addgroup -S -g 10001 app && adduser -S -u 10001 -G app app
RUN mkdir -p /data/avatars && chown -R 10001:10001 /data
COPY --from=build /app/target/avatar-service.jar app.jar
USER 10001:10001
ENV PORT=8090 \
    UPLOAD_DIR=/data/avatars
EXPOSE 8090
ENTRYPOINT ["java", "-jar", "/app/app.jar"]
