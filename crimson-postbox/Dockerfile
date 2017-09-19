FROM node:6.9.1-alpine
RUN apk update && apk upgrade
RUN apk add git curl openssh
RUN rm -rf /var/cache/apk/*
# Install JDK - see https://github.com/docker-library/openjdk/
ENV LANG C.UTF-8

RUN { \
		echo '#!/bin/sh'; \
		echo 'set -e'; \
		echo; \
		echo 'dirname "$(dirname "$(readlink -f "$(which javac || which java)")")"'; \
	} > /usr/local/bin/docker-java-home \
	&& chmod +x /usr/local/bin/docker-java-home
ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk
ENV PATH $PATH:/usr/lib/jvm/java-1.8-openjdk/jre/bin:/usr/lib/jvm/java-1.8-openjdk/bin

ENV JAVA_VERSION 8u111
ENV JAVA_ALPINE_VERSION 8.111.14-r0

RUN set -x \
	&& apk add --no-cache \
		openjdk8="$JAVA_ALPINE_VERSION" \
	&& [ "$JAVA_HOME" = "$(docker-java-home)" ]

# Maven installation - see https://github.com/carlossg/docker-maven/

ARG MAVEN_VERSION=3.3.9
ARG USER_HOME_DIR="/root"


RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL http://apache.osuosl.org/maven/maven-3/$MAVEN_VERSION/binaries/apache-maven-$MAVEN_VERSION-bin.tar.gz \
    | tar -xzC /usr/share \
  && ln -s /usr/share/apache-maven-$MAVEN_VERSION/bin/mvn /usr/bin/mvn

ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG "$USER_HOME_DIR/.m2"

RUN git clone https://github.com/ewolff/crimson-postbox.git
RUN cd crimson-postbox
RUN cd crimson-postbox && npm install
RUN cd crimson-postbox && npm run compile
RUN cd crimson-postbox && mvn clean install
CMD cd crimson-postbox && mvn spring-boot:run
EXPOSE 3001
