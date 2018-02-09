FROM jetty:9.4.7-alpine

ENV RDF4J_VERSION "2.2.2"
ENV MDP_VERSION "3.0.2"

USER root

WORKDIR /tmp

RUN apk --update --no-cache -X http://mirror.clarkson.edu/alpine/edge/community/ add maven && \
    mvn org.apache.maven.plugins:maven-dependency-plugin:${MDP_VERSION}:copy -Dartifact=org.eclipse.rdf4j:rdf4j-http-server:${RDF4J_VERSION}:war -DoutputDirectory=`/bin/pwd` -Dmdep.stripVersion=true && \
    mvn org.apache.maven.plugins:maven-dependency-plugin:${MDP_VERSION}:copy -Dartifact=org.eclipse.rdf4j:rdf4j-http-workbench:${RDF4J_VERSION}:war -DoutputDirectory=`/bin/pwd` -Dmdep.stripVersion=true && \
    mv rdf4j-http-server.war /var/lib/jetty/webapps/rdf4j-server.war && \
    mv rdf4j-http-workbench.war /var/lib/jetty/webapps/rdf4j-workbench.war && \
    chown jetty:jetty /var/lib/jetty/webapps/rdf4j-* && \
    rm -rf ~/.m2 && \
    apk del maven && \
    rm -rf /var/cache/apk/* && \
    rm -rf /var/lib/apk/*  && \
    rm -rf hsperfdata_root

WORKDIR $JETTY_BASE

USER jetty