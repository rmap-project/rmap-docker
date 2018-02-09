FROM jetty:9.4.7-alpine

ENV RMAP_VERSION "1.0.1-beta-SNAPSHOT"
ENV MDP_VERSION "3.0.2"

USER root

WORKDIR /tmp

ADD settings.xml settings.xml

RUN apk --update --no-cache -X http://mirror.clarkson.edu/alpine/edge/community/ add maven && \
    mvn -s ./settings.xml org.apache.maven.plugins:maven-dependency-plugin:${MDP_VERSION}:copy -Dartifact=info.rmapproject:rmap-api:${RMAP_VERSION}:war -DoutputDirectory=`/bin/pwd` -Dmdep.stripVersion=true && \
    mv rmap-api.war /var/lib/jetty/webapps/api.war && \
    chown jetty:jetty /var/lib/jetty/webapps/api.war && \
    rm -rf ~/.m2 && \
    apk del maven && \
    rm -rf /var/cache/apk/* && \
    rm -rf /var/lib/apk/*  && \
    rm -rf hsperfdata_root

WORKDIR $JETTY_BASE

USER jetty