FROM tomcat:10-jdk21

ARG JOLOKIA_VERSION=2.4.2

# Download Jolokia WAR for proxy mode (unsecured variant)
RUN wget -O /usr/local/tomcat/webapps/jolokia.war \
    "https://repo1.maven.org/maven2/org/jolokia/jolokia-agent-war-unsecured/${JOLOKIA_VERSION}/jolokia-agent-war-unsecured-${JOLOKIA_VERSION}.war"

EXPOSE 8080

CMD ["catalina.sh", "run"]
