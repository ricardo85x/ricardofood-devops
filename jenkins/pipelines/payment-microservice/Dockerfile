FROM openjdk:17.0.1-buster

ENV APP_HOME /usr/src/app

COPY target/payment.jar $APP_HOME/app.jar

WORKDIR $APP_HOME

ENTRYPOINT ["java","-jar","app.jar", "--spring.profiles.active=prod"]
