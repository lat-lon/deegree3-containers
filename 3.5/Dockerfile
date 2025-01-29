FROM bitnami/tomcat:9.0.93-debian-12-r1

# set deegree version
ARG DEEGREE_VERSION=3.5.10
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
ENV LANG en_US.UTF-8

USER root

# Reinstall zlib1g with fixed CVE-2023-45853 on trixie: https://security-tracker.debian.org/tracker/CVE-2023-45853
RUN echo 'deb http://deb.debian.org/debian trixie main' > /etc/apt/sources.list.d/trixie.list \
    && echo $'Package: *\nPin: release n=trixie\nPin-Priority: -10\n\nPackage: zlib1g\nPin: release n=trixie\nPin-Priority: 501' > /etc/apt/preferences.d/trixie.pref \
    && apt-get update && apt-get -u install zlib1g && apt-get -y install curl

# download deegree webservices WAR file
RUN curl https://repo.deegree.org/content/repositories/public/org/deegree/deegree-webservices/${DEEGREE_VERSION}/deegree-webservices-${DEEGREE_VERSION}.war -o /tmp/deegree-webservices.war
RUN mkdir /opt/bitnami/tomcat/webapps/$WEB_CONTEXT \
	&& cd /opt/bitnami/tomcat/webapps/$WEB_CONTEXT \
	&& jar xf /tmp/deegree-webservices.war \
	&& rm /tmp/deegree-webservices.war

RUN chmod o+w /opt/bitnami/tomcat/bin/setenv.sh \
    && chmod o+w /opt/bitnami/tomcat/logs/ \
	&& chmod o+w /opt/bitnami/tomcat/work/ \
	&& chmod o+w /opt/bitnami/tomcat/temp \
	&& chmod o+w /opt/bitnami/tomcat/webapps \
    && mkdir -p $DEEGREE_WORKSPACE_ROOT && chown 1001 $DEEGREE_WORKSPACE_ROOT \
    && mkdir -p $LOG_DIR && chmod a+w $LOG_DIR

USER 1001
