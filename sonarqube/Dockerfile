# Use an official Alpine Linux image as the base image
FROM alpine:${ALPINE_VERSION} as build

ARG ALPINE_VERSION
ARG JAVA_VERSION
ARG SONARQUBE_VERSION
ARG BUILD_NUMBER

LABEL org.opencontainers.images.authors="fdjapi@gmail.com"
LABEL ALPINE_VERSION=${ALPINE_VERSION}
LABEL JAVA_VERSION=${JAVA_VERSION}
LABEL SONARQUBE_VERSION=${SONARQUBE_VERSION}

# Set up the repositories
RUN echo "http://dl-cdn.alpinelinux.org/alpine/v${ALPINE_VERSION}/main" > /etc/apk/repositories
RUN echo "http://dl-cdn.alpinelinux.org/alpine/v${ALPINE_VERSION}/community" >> /etc/apk/repositories

# Update the package list
RUN apk update

# Download OpenJDK
RUN wget -O /tmp/openjdk.tar.gz "https://download.java.net/java/GA/jdk${JAVA_VERSION}/${BUILD_NUMBER}/GPL/openjdk-${JAVA_VERSION}_linux-x64_bin.tar.gz"

# Extract OpenJDK and clean up
RUN tar -xf /tmp/openjdk.tar.gz -C /opt/
RUN rm /tmp/openjdk.tar.gz

# Set JAVA_HOME and update PATH
ENV JAVA_HOME=/opt/jdk-${JAVA_VERSION}
ENV PATH="$JAVA_HOME/bin:$PATH"

# Download SonarQube
RUN wget -O /opt/sonarqube.zip "https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-${SONARQUBE_VERSION}.zip"

# Install Bash and unzip
RUN apk add --no-cache bash unzip
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
