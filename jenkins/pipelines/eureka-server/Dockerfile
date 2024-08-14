FROM openjdk:17.0.1-buster

ENV APP_HOME /usr/src/app

EXPOSE 8081

COPY target/eureka_server.jar $APP_HOME/app.jar

WORKDIR $APP_HOME

ENTRYPOINT ["java","-jar","app.jar"]
