FROM bitnami/tomcat:9.0.87-debian-12-r0

ARG BUILD_DATE
ARG VCS_REF
ARG VCS_URL
LABEL maintainer="lat/lon <info@lat-lon.de>" \
	"org.opencontainers.image.vendor"="lat/lon GmbH" \
    "org.opencontainers.image.created"="$BUILD_DATE" \
    "org.opencontainers.image.source"="$VCS_URL" \
    "org.opencontainers.image.revision"="$VCS_REF"

# set deegree version
ARG DEEGREE_VERSION=3.5.6
ENV TOMCAT_PASSWORD=deegree
ENV LOG_DIR=/deegree/logs
ENV DEEGREE_WORKSPACE_ROOT=/deegree/workspaces
ENV TOMCAT_EXTRA_JAVA_OPTS="-Djavax.xml.transform.TransformerFactory=net.sf.saxon.TransformerFactoryImpl -Dlog.dir=$LOG_DIR --add-opens java.base/java.lang=ALL-UNNAMED --add-opens java.desktop/com.sun.imageio.spi=ALL-UNNAMED --add-opens java.desktop/javax.imageio.spi=ALL-UNNAMED --add-exports java.desktop/sun.awt=ALL-UNNAMED --add-exports java.desktop/com.sun.imageio.spi=ALL-UNNAMED --add-exports java.desktop/sun.swing=ALL-UNNAMED --add-opens java.desktop/javax.imageio.spi=ALL-UNNAMED"
ENV LANG en_US.UTF-8

USER root

RUN echo 'deb http://deb.debian.org/debian sid main' > /etc/apt/sources.list.d/sid.list \
    && echo $'Package: *\nPin: release n=sid\nPin-Priority: -10\n\nPackage: zlib1g\nPin: release n=sid\nPin-Priority: 501' > /etc/apt/preferences.d/sid.pref \
    && apt-get update && apt-get -u install zlib1g && apt-get -y install curl

WORKDIR /opt/bitnami/tomcat/webapps/
# download deegree webservices WAR file
RUN curl https://repo.deegree.org/content/repositories/public/org/deegree/deegree-webservices/${DEEGREE_VERSION}/deegree-webservices-${DEEGREE_VERSION}.war -o /opt/bitnami/tomcat/webapps/deegree-webservices.war

RUN chmod o+w /opt/bitnami/tomcat/bin/setenv.sh \
	&& chmod o+w /opt/bitnami/tomcat/logs/ \
	&& chmod o+w /opt/bitnami/tomcat/conf /opt/bitnami/tomcat/conf/* \
	&& chmod o+w /opt/bitnami/tomcat/work/ \
	&& chmod o+w /opt/bitnami/tomcat/temp \
    && mkdir -p $DEEGREE_WORKSPACE_ROOT && chown 1001 $DEEGREE_WORKSPACE_ROOT \
    && mkdir -p $LOG_DIR && chmod a+w $LOG_DIR

USER 1001