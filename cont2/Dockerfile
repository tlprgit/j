FROM tomcat
MAINTAINER TLPR 123
WORKDIR /usr/local/tomcat/webapps
RUN mkdir ROOT
COPY ./cont2.html /usr/local/tomcat/webapps/ROOT
EXPOSE 8080
CMD ['catalina.sh','run']