FROM bitnami/tomcat:10.1-debian-12 AS builder

ARG DEEGREE_VERSION=3.5.13
ARG TC_MIG_VERSION=1.0.9

USER root

RUN apt-get update && apt-get -y install curl \
    && mkdir -p /build_deegree \
    && cd /build_deegree \
    && curl https://repo.deegree.org/content/repositories/public/org/deegree/deegree-webservices/${DEEGREE_VERSION}/deegree-webservices-${DEEGREE_VERSION}.war -o /build_deegree/deegree-webservices-original.war \
    && curl -o jakartaee-migration-${TC_MIG_VERSION}-shaded.jar https://dlcdn.apache.org/tomcat/jakartaee-migration/v${TC_MIG_VERSION}/binaries/jakartaee-migration-${TC_MIG_VERSION}-shaded.jar \
    && java -jar jakartaee-migration-${TC_MIG_VERSION}-shaded.jar /build_deegree/deegree-webservices-original.war /build_deegree/deegree-webservices.war

FROM bitnami/tomcat:10.1-debian-12 AS runner

ARG BUILD_DATE
ARG VCS_REF
ARG VCS_URL
ARG WEB_CONTEXT=deegree-webservices
LABEL maintainer="lat/lon <info@lat-lon.de>" \
	"org.opencontainers.image.vendor"="lat/lon GmbH" \
    "org.opencontainers.image.created"="$BUILD_DATE" \
    "org.opencontainers.image.source"="$VCS_URL" \
    "org.opencontainers.image.revision"="$VCS_REF"

ENV TOMCAT_PASSWORD=deegree
ENV LOG_DIR=/deegree/logs
ENV DEEGREE_WORKSPACE_ROOT=/deegree/workspaces
ENV TOMCAT_EXTRA_JAVA_OPTS="-Djavax.xml.transform.TransformerFactory=net.sf.saxon.TransformerFactoryImpl -Dlog.dir=$LOG_DIR --add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.desktop/com.sun.imageio.spi=ALL-UNNAMED --add-opens java.desktop/javax.imageio.spi=ALL-UNNAMED --add-exports java.desktop/sun.awt=ALL-UNNAMED --add-exports java.desktop/com.sun.imageio.spi=ALL-UNNAMED --add-exports java.desktop/sun.swing=ALL-UNNAMED --add-opens java.desktop/javax.imageio.spi=ALL-UNNAMED"
ENV LANG=en_US.UTF-8

USER root

# Reinstall zlib1g with fixed CVE-2023-45853 on trixie: https://security-tracker.debian.org/tracker/CVE-2023-45853
RUN echo 'deb http://deb.debian.org/debian trixie main' > /etc/apt/sources.list.d/trixie.list \
    && echo $'Package: *\nPin: release n=trixie\nPin-Priority: -10\n\nPackage: zlib1g\nPin: release n=trixie\nPin-Priority: 501' > /etc/apt/preferences.d/trixie.pref \
    && apt-get update && apt-get -u install zlib1g && apt-get -y install unzip fontconfig

COPY --from=builder /build_deegree/deegree-webservices.war /tmp/deegree-webservices.war
RUN mkdir /opt/bitnami/tomcat/webapps/$WEB_CONTEXT \
	&& cd /opt/bitnami/tomcat/webapps/$WEB_CONTEXT \
    && unzip '/tmp/deegree-webservices.war' \
    && rm /tmp/deegree-webservices.war

RUN chmod o+w /opt/bitnami/tomcat/bin/setenv.sh \
    && chmod o+w /opt/bitnami/tomcat/logs/ \
	&& chmod o+w /opt/bitnami/tomcat/work/ \
	&& chmod o+w /opt/bitnami/tomcat/temp \
	&& chmod o+w /opt/bitnami/tomcat/webapps \
    && mkdir -p $DEEGREE_WORKSPACE_ROOT && chown 1001 $DEEGREE_WORKSPACE_ROOT \
    && mkdir -p $LOG_DIR && chmod a+w $LOG_DIR

WORKDIR /opt/bitnami/tomcat
USER 1001
