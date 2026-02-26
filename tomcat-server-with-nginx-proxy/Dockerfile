FROM alpine:latest

LABEL maintainer="ANURAG MISHRA | anu@gmail.com"

RUN apk update && apk add --no-cache openjdk17-jdk bash curl tar

COPY Dockerfile /root/

WORKDIR /opt/

ADD  apache-tomcat-9.0.115.tar.gz    /opt/

RUN chmod +x /opt/apache-tomcat-9.0.115/bin/catalina.sh

EXPOSE 8080

CMD ["/opt/apache-tomcat-9.0.115/bin/catalina.sh","run"]
