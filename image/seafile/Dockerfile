FROM seafileltd/base-mc:18.04
WORKDIR /opt/seafile

RUN mkdir -p /etc/my_init.d

ENV SEAFILE_VERSION=6.3.4 SEAFILE_SERVER=seafile-server

RUN mkdir -p /opt/seafile/ && \
    curl -sSL -o - https://download.seadrive.org/seafile-server_${SEAFILE_VERSION}_x86-64.tar.gz \
    | tar xzf - -C /opt/seafile/

# For using TLS connection to LDAP/AD server with docker-ce.
RUN find /opt/seafile/ \( -name "liblber-*" -o -name "libldap-*" -o -name "libldap_r*" -o -name "libsasl2.so*" \) -delete

ADD scripts/create_data_links.sh /etc/my_init.d/01_create_data_links.sh

COPY scripts /scripts
COPY templates /templates

EXPOSE 80

# RUN apt-get update && apt-get install sudo
RUN useradd -u 1000 -r -g root -m -d /seafile -s /sbin/nologin -c "App user" seafile && \
    chmod 755 /seafile
#    chown -R seafile:root /etc/nginx/sites-enabled/ && \
#    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers && adduser seafile sudo
# USER seafile
# RUN cat /etc/sudoers
# RUN sudo uname -a
# USER root

CMD /sbin/my_init -- /bin/bash -c '/scripts/start.py && setuser seafile /scripts/run.py'
