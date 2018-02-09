FROM jetty:9.4.7-alpine

ENV RMAP_VERSION "1.0.1-beta-SNAPSHOT"
ENV POSTGRES_VERSION "42.2.1.jre7"
ENV MDP_VERSION "3.0.2"

USER root

WORKDIR /tmp

ADD settings.xml settings.xml

ADD rmap.properties /var/lib/jetty/lib/ext/rmap.properties

RUN apk --update --no-cache -X http://mirror.clarkson.edu/alpine/edge/community/ add maven && \
    mvn -s ./settings.xml org.apache.maven.plugins:maven-dependency-plugin:${MDP_VERSION}:copy -Dartifact=info.rmapproject:rmap-webapp:${RMAP_VERSION}:war -DoutputDirectory=`/bin/pwd` -Dmdep.stripVersion=true && \
    mvn -s ./settings.xml org.apache.maven.plugins:maven-dependency-plugin:${MDP_VERSION}:copy -Dartifact=info.rmapproject:rmap-api:${RMAP_VERSION}:war -DoutputDirectory=`/bin/pwd` -Dmdep.stripVersion=true && \
    mvn -s ./settings.xml org.apache.maven.plugins:maven-dependency-plugin:${MDP_VERSION}:copy -Dartifact=org.postgresql:postgresql:${POSTGRES_VERSION}:jar -DoutputDirectory=`/bin/pwd` -Dmdep.stripVersion=true && \
    mv rmap-api.war /var/lib/jetty/webapps/api.war && \
    chown jetty:jetty /var/lib/jetty/webapps/api.war && \
    mv rmap-webapp.war /var/lib/jetty/webapps/app.war && \
    chown jetty:jetty /var/lib/jetty/webapps/app.war && \
    mv postgresql.jar /var/lib/jetty/lib/ext/ && \
    chown jetty:jetty /var/lib/jetty/lib/ext/postgresql.jar && \
    chown jetty:jetty /var/lib/jetty/lib/ext/rmap.properties && \
    rm -rf ~/.m2 && \
    apk del maven && \
    rm -rf /var/cache/apk/* && \
    rm -rf /var/lib/apk/*  && \
    rm -rf hsperfdata_root

WORKDIR $JETTY_BASE

USER jetty