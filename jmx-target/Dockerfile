FROM eclipse-temurin:21-jdk AS build

WORKDIR /app

# Create a simple Java application that just sleeps (for JMX monitoring)
RUN echo 'public class JmxTarget { public static void main(String[] args) throws Exception { System.out.println("JMX Target running..."); while(true) { Thread.sleep(1000); } } }' > JmxTarget.java \
    && javac JmxTarget.java

FROM eclipse-temurin:21-jre

WORKDIR /app
COPY --from=build /app/JmxTarget.class .

EXPOSE 9999

# Enable JMX remote access
CMD ["java", \
     "-Dcom.sun.management.jmxremote", \
     "-Dcom.sun.management.jmxremote.port=9999", \
     "-Dcom.sun.management.jmxremote.rmi.port=9999", \
     "-Dcom.sun.management.jmxremote.local.only=false", \
     "-Dcom.sun.management.jmxremote.authenticate=false", \
     "-Dcom.sun.management.jmxremote.ssl=false", \
     "-Djava.rmi.server.hostname=jmx-target", \
     "JmxTarget"]
