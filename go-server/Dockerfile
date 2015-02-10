# Dockerfile for ThoughtWorks Go Server (http://www.go.cd)
#
# http://github.com/patforna/docker/go-serverU

FROM dockerfile/java
MAINTAINER Javier Pérez <surtich@gmail.com>

ENV GO_DOWNLOAD_URL http://download.go.cd/gocd-deb/go-server-14.4.0-1356.deb

RUN wget -O /tmp/go-server.deb ${GO_DOWNLOAD_URL}
RUN dpkg -i /tmp/go-server.deb
RUN rm -f /tmp/go-server.deb

RUN mkdir -p /var/lib/go-server/db/h2db /etc/go /var/lib/go-server/db/config.git

COPY db.zip /tmp/
COPY config-dir.zip /tmp/
COPY config-repo.zip /tmp/

RUN unzip -o /tmp/db.zip -d /var/lib/go-server/db/h2db/
RUN rm -f /tmp/db.zip

RUN unzip -o /tmp/config-dir.zip -d /etc/go/
RUN rm -f /tmp/config-dir.zip
 
RUN unzip -o /tmp/config-repo.zip -d /var/lib/go-server/db/config.git/
RUN rm -f /tmp/config-repo.zip

RUN chown -R go:go /var/lib/go-server/db /etc/go

CMD /etc/init.d/go-server start && tail -F /var/log/go-server/go-server.log

EXPOSE 8153
EXPOSE 8154
