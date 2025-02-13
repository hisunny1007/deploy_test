# 빌드 스테이지
# 베이스 이미지
# 의존성 설치
# 소스 코드 복사 & 빌드

# 실행 이미지
# jar 파일 복사
# jar 파일 실행

# [Build stage]
FROM openjdk:21-jdk-slim AS build
# 컨테이너 내부 작업 디렉토리 설정
WORKDIR /app

## 의존성 캐싱 레이어
COPY build.gradle settings.gradle ./
COPY gradle ./gradle
COPY gradlew ./gradlew
# gradlew 파일 실행 권한 부여
RUN chmod +x gradlew
# 의존성 다운로드 (캐시 활용을 위해 소스 코드 복사 전에 실행)
RUN ./gradlew dependencies --no-daemon

## 소스 코드 복사 및 프로젝트 빌드
COPY src ./src
RUN ./gradlew build --no-daemon -x test

# [Run stage]
FROM eclipse-temurin:21-jre-alpine
WORKDIR /app

# 빌드 단계에서 생성된 JAR 파일 복사
COPY --from=build /app/build/libs/*.jar app.jar

# Spring Boot 프로젝트 포트 노출
EXPOSE 8080

# 프로젝트 실행 명령
ENTRYPOINT ["java", "-jar", "app.jar"]


