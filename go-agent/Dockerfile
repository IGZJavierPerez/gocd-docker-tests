# Dockerfile for ThoughtWorks Go Agent (http://www.go.cd)
#
# http://github.com/patforna/docker/go-agent

FROM dockerfile/java
MAINTAINER Javier Pérez <surtich@gmail.com>

ENV GO_DOWNLOAD_URL http://download.go.cd/gocd-deb/go-agent-14.4.0-1356.deb

RUN wget -O /tmp/go-agent.deb ${GO_DOWNLOAD_URL}
RUN dpkg -i /tmp/go-agent.deb
RUN rm -f /tmp/go-agent.deb

# get rid of GO_SERVER AND GO_SERVER_PORT defaults. We'll use env variables.
RUN sed -i '/.*GO_SERVER.*/d' /etc/default/go-agent

# make sure container doesn't exit after starting agent
RUN sed -i 's/DAEMON=Y/DAEMON=N/' /etc/default/go-agent

# make sure that init.d script passes through environment variables
RUN sed -i 's/su -/su -p/' /etc/init.d/go-agent

# set GO_SERVER env variable to go-server (defined in /etc/hosts)
ENV GO_SERVER go-server
ENV GO_SERVER_PORT 8153

# Let's start with some basic stuff.
RUN apt-get update -qq && apt-get install -qqy \
    apt-transport-https \
    ca-certificates \
    curl \
    lxc \
    iptables
    
# Install Docker from Docker Inc. repositories.
RUN curl -sSL https://get.docker.com/ubuntu/ | sh

# Install the magic wrapper.
ADD ./wrapdocker /usr/local/bin/wrapdocker
RUN chmod +x /usr/local/bin/wrapdocker

# Define additional metadata for our image.
VOLUME /var/lib/docker

# This file has my authorization string so that I don't have to approve new agents. 
ADD autoregister.properties /var/lib/go-agent/config/autoregister.properties

CMD su go -c '/etc/init.d/go-agent start'
