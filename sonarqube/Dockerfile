FROM alpine:${ALPINE_VERSION} as build
ARG JAVA_VERSION
ARG SONARQUBE_VERSION
LABEL org.opencontainers.images.authors="fdjapi@gmail.com"
LABEL JAVA_VERSION=${JAVA_VERSION}
LABEL SONARQUBE_VERSION=${SONARQUBE_VERSION}
RUN apk update \
 && apk add --no-cache openjdk${JAVA_VERSION} \
 && wget -q -O /tmp/sonarqube.zip "https://binaries.sonarsource.com/commercialDistribution/sonarqube-enterprise-${SONARQUBE_VERSION}.zip" \
 && unzip -q /tmp/sonarqube.zip -d /opt \
 && rm /tmp/sonarqube.zip \
 && rm -rf ${SONARQUBE_HOME}/bin/linux-x86-64/*.bat
FROM alpine:${ALPINE_VERSION}
ENV SONARQUBE_HOME=/opt/sonarqube-${SONARQUBE_VERSION}
COPY --from=build ${SONARQUBE_HOME} ${SONARQUBE_HOME}
ADD sonar.properties ${SONARQUBE_HOME}/conf/sonar.properties
ENV SONAR_HOST_URL=
ENV SONAR_JDBC_URL=
ENV SONAR_JDBC_USERNAME=
ENV SONAR_JDBC_PASSWORD=
ENV SONAR_SECURITY_REALM=
ENV SONAR_SECURITY_SAVEPASSWORD=
ENV SONAR_TYPESCRIPT_LCOV_REPORTPATHS=
ENTRYPOINT ["${SONARQUBE_HOME}/bin/linux-x86-64/sonar.sh", "start"]
