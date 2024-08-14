FROM openjdk:17.0.1-buster

ENV APP_HOME /usr/src/app

EXPOSE 8082

COPY target/gateway.jar $APP_HOME/app.jar

WORKDIR $APP_HOME

ENTRYPOINT ["java","-jar","app.jar"]
