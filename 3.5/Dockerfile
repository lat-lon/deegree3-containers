FROM alpine:3.22 AS builder

ARG DEEGREE_VERSION=3.5.15
ARG TC_MIG_VERSION=1.0.9

USER root

RUN apk --no-cache add curl \
    && mkdir -p /build_deegree \
    && cd /build_deegree \
    && curl https://repo.deegree.org/content/repositories/public/org/deegree/deegree-webservices/${DEEGREE_VERSION}/deegree-webservices-${DEEGREE_VERSION}.war -o /build_deegree/deegree-webservices.war

FROM tomcat:9.0-jdk11-temurin-noble AS runner

ARG BUILD_DATE
ARG VCS_REF
ARG VCS_URL
ARG WEB_CONTEXT=deegree-webservices
LABEL maintainer="lat/lon <info@lat-lon.de>" \
	"org.opencontainers.image.vendor"="lat/lon GmbH" \
    "org.opencontainers.image.created"="$BUILD_DATE" \
    "org.opencontainers.image.source"="$VCS_URL" \
    "org.opencontainers.image.revision"="$VCS_REF"

ENV LOG_DIR=/deegree/logs
ENV DEEGREE_WORKSPACE_ROOT=/deegree/workspaces
ENV TOMCAT_EXTRA_JAVA_OPTS="-Djavax.xml.transform.TransformerFactory=net.sf.saxon.TransformerFactoryImpl -Dlog.dir=$LOG_DIR --add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.desktop/com.sun.imageio.spi=ALL-UNNAMED --add-opens java.desktop/javax.imageio.spi=ALL-UNNAMED --add-exports java.desktop/sun.awt=ALL-UNNAMED --add-exports java.desktop/com.sun.imageio.spi=ALL-UNNAMED --add-exports java.desktop/sun.swing=ALL-UNNAMED --add-opens java.desktop/javax.imageio.spi=ALL-UNNAMED"
ENV LANG=en_US.UTF-8

USER root

RUN apt-get update && apt-get -y install unzip fontconfig

COPY --from=builder /build_deegree/deegree-webservices.war /tmp/deegree-webservices.war
RUN mkdir /usr/local/tomcat/webapps/$WEB_CONTEXT \
	&& cd /usr/local/tomcat/webapps/$WEB_CONTEXT \
    && unzip '/tmp/deegree-webservices.war' \
    && rm /tmp/deegree-webservices.war

#RUN chmod o+w /usr/local/tomcat/bin/setenv.sh
RUN  chmod o+w /usr/local/tomcat/logs/
RUN  chmod o+w /usr/local/tomcat/work/
RUN  chmod o+w /usr/local/tomcat/temp \
	&& chmod o+w /usr/local/tomcat/webapps \
    && mkdir -p $DEEGREE_WORKSPACE_ROOT && chown 1001 $DEEGREE_WORKSPACE_ROOT \
    && mkdir -p $LOG_DIR && chmod a+w $LOG_DIR

WORKDIR /usr/local/tomcat
USER 1001
