FROM tomcat:10-jdk21

ARG JOLOKIA_VERSION=2.4.2

# Download and pre-expand Jolokia WAR for proxy mode (unsecured variant)
# Pre-expanding avoids runtime directory creation issues with read-only filesystems
RUN wget -O /tmp/jolokia.war \
    "https://repo1.maven.org/maven2/org/jolokia/jolokia-agent-war-unsecured/${JOLOKIA_VERSION}/jolokia-agent-war-unsecured-${JOLOKIA_VERSION}.war" && \
    mkdir -p /usr/local/tomcat/webapps/jolokia && \
    cd /usr/local/tomcat/webapps/jolokia && \
    jar -xf /tmp/jolokia.war && \
    rm /tmp/jolokia.war

# Pre-create config directory that Tomcat expects to write to
RUN mkdir -p /usr/local/tomcat/conf/Catalina/localhost

EXPOSE 8080

CMD ["catalina.sh", "run"]
