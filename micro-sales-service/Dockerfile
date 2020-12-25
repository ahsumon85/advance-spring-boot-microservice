FROM openjdk:8
VOLUME /app/log
ADD target/drug-prod-ms-rs-0.0.1-SNAPSHOT.jar drug-prod-ms-rs-0.0.1-SNAPSHOT.jar
EXPOSE 8380
ENTRYPOINT ["java", "-jar", "drug-prod-ms-rs-0.0.1-SNAPSHOT.jar"]
