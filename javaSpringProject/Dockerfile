FROM amazoncorretto

EXPOSE 8080

WORKDIR /mydir

COPY spring-example-project/ .

RUN ./mvnw package

CMD ["java", "-jar",  "./target/docker-example-1.1.3.jar"]
