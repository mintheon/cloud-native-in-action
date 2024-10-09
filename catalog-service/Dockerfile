# 첫번째 단계
FROM eclipse-temurin:17 AS builder
WORKDIR workspace

ARG JAR_FILE=build/libs/*.jar
COPY ${JAR_FILE} catalog-service.jar
RUN java -Djarmode=layertools -jar catalog-service.jar extract

# 두번째 단계
FROM eclipse-temurin:17
RUN useradd spring
USER spring
WORKDIR workspace
COPY --from=builder workspace/dependencies/ ./
COPY --from=builder workspace/spring-boot-loader/ ./
COPY --from=builder workspace/snapshot-dependencies/ ./
COPY --from=builder workspace/application/ ./

# 스프링 부트 런처를 사용해 우버 jar 대신 계층으로 애플리케이션을 시작
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]