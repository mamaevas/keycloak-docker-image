FROM openjdk:8-jdk-alpine

MAINTAINER mamaevas

ENV KEYCLOAK_ADMIN_USER=admin
ENV KEYCLOAK_ADMIN_PASSWORD=admin

USER root

# Prepare infrastructure to install keycloak
RUN apk update \
	&& apk upgrade \
	&& apk add bash \
	&& apk add curl \
	&& mkdir /opt

# Use this to download keycloak and extract it on the fly to /opt directory
RUN curl https://downloads.jboss.org/keycloak/4.0.0.Final/keycloak-4.0.0.Final.tar.gz | tar xzf - --directory /opt

# Add admin to the keycloak (for access to the admin UI)
# Uncomment if admin still not exists in db
RUN ./opt/keycloak-4.0.0.Final/bin/add-user-keycloak.sh -r master -u $KEYCLOAK_ADMIN_USER -p $KEYCLOAK_ADMIN_PASSWORD

# Add external DB driver(add mariaDB jdbc driver and module.xml into keycloak)
COPY mariadb-java-client-2.2.5.jar ./opt/keycloak-4.0.0.Final/modules/system/layers/keycloak/org/mariadb/main/mariadb-java-client-2.2.5.jar
COPY module.xml ./opt/keycloak-4.0.0.Final/modules/system/layers/keycloak/org/mariadb/main/module.xml

# Replace standalone.xml (add external datasource to keycloak configuration)
RUN mv ./opt/keycloak-4.0.0.Final/standalone/configuration/standalone.xml ./opt/keycloak-4.0.0.Final/standalone/configuration/standalone-old.xml
COPY standalone.xml ./opt/keycloak-4.0.0.Final/standalone/configuration/standalone.xml

EXPOSE 8080

ENTRYPOINT ["./opt/keycloak-4.0.0.Final/bin/standalone.sh"]

CMD ["-b", "0.0.0.0"]
