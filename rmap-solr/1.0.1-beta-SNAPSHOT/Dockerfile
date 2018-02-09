FROM solr:6.6.2-alpine

ENV SOLR_PORT 8983
ENV SOLR_DEBUG_PORT 5005
ENV RMAP_VERSION "1.0.1-beta-SNAPSHOT"
ENV MDP_VERSION "3.0.2"

USER root

WORKDIR /tmp

ADD settings.xml settings.xml

RUN apk --update --no-cache -X http://mirror.clarkson.edu/alpine/edge/community/ add maven openjdk8 && \
    mvn -s ./settings.xml org.apache.maven.plugins:maven-dependency-plugin:${MDP_VERSION}:copy -Dartifact=info.rmapproject:rmap-indexing-solr-resources:${RMAP_VERSION}:jar -DoutputDirectory=`/bin/pwd` -Dmdep.stripVersion=true && \
    (cd /opt/solr/server/solr/mycores && jar -xf /tmp/rmap-indexing-solr-resources.jar) && \
    chown -R ${SOLR_USER} /opt/solr/server/solr/mycores && \
    rm -rf ~/.m2 && \
    apk del maven openjdk8 && \
    rm -rf /var/cache/apk/* && \
    rm -rf /var/lib/apk/*  && \
    rm -rf hsperfdata_root

WORKDIR /opt/solr

USER $SOLR_USER