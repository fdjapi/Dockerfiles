FROM fresh as build 
ARG ALPINE_VERSION
ARG JAVA_VERSION
ARG SONARQUBE_VERSION
ARG BUILD_NUMBER
LABEL org.opencontainers.images.authors="fdjapi@gmail.com"
LABEL ALPINE_VERSION=${ALPINE_VERSION}
LABEL JAVA_VERSION=${JAVA_VERSION}
LABEL SONARQUBE_VERSION=${SONARQUBE_VERSION}
RUN echo "http://dl-cdn.alpinelinux.org/alpine/v${ALPINE_VERSION}/main > /etc/apk/repositories"
RUN echo "http://dl-cdn.alpinelinux.org/alpine/v${ALPINE_VERSION}/community >> /etc/apk/repositories"
RUN apk update
RUN wget -O /tmp/openjdk.tar.gz "https://download.java.net/openjdk/jdk${JAVA_VERSION}/ri/openjdk-${JAVA_VERSION}+${BUILD_NUMBER}_linux-x64_bin.tar.gz"
RUN tar -xf /tmp/openjdk.tar.gz -C /opt/
RUN rm /tmp/openjdk.tar.gz
ENV JAVA_HOME=/opt/jdk-${JAVA_VERSION}
ENV PATH="$JAVA_HOME/bin:$PATH"
RUN wget -O /opt/sonarqube.zip "https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-${SONARQUBE_VERSION}.zip"
RUN apk add bash
RUN unzip /opt/sonarqube.zip -d /opt/
RUN rm /opt/sonarqube.zip
FROM build
ENV SONARQUBE_HOME=/opt/sonarqube-${SONARQUBE_VERSION}
ADD sonar.properties  ${SONARQUBE_HOME}/conf/sonar.properties
ENV SONAR_HOST_URL=
ENV SONAR_JDBC_URL=
ENV SONAR_JDBC_USERNAME=
ENV SONAR_JDBC_PASSWORD=
ENV SONAR_SECURITY_REALM=
ENV SONAR_SECURITY_SAVEPASSWORD=
ENV SONAR_TYPESCRIPT_LCOV_REPORTPATHS=
ENTRYPOINT ["${SONARQUBE_HOME}/bin/linux-x86-64/sonar.sh", "start"]
