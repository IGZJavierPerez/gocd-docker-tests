To start a dockerized go-server, open a terminal and type:

```bash
cd go-server
docker build -t go-server .
docker run --name go-server -it --rm -p 8153:8153 go-server
```

Navigate to [http://localhost:8153/](http://localhost:8153/)

To start a dockerized go-agent, open a terminal and type:

```bash
cd go-agent
docker build -t go-agent .
docker run  -it --rm --link go-server:go-server --name go-agent --privileged go-agent bash -c "PORT=1234 wrapdocker && /etc/init.d/go-agent start"
#Bind both Docker: docker run  -it --rm --link go-server:go-server --name go-agent --privileged -v /var/lib/docker:/var/lib/docker go-agent bash -c "PORT=1234 wrapdocker && /etc/init.d/go-agent start"
```

[Check](http://localhost:8153/go/agents?column=status&order=ASC) the agent has been auto registered.

Trigger the [Deploy pipeline]

If it fails because a conflict name, open a terminal and execute:

```bash
docker exec -it go-agent bash
docker --host=tcp://localhost:1234  rm -f $(docker --host=tcp://localhost:1234 ps -q -a)
```

Then, restart the pipeline.

To find out go-agent IP, open a terminal and execute:

```bash
docker inspect --format '{{ .NetworkSettings.IPAddress }}' go-agent
```

Navigate to: http://[go-agent ip]:8000

To start a stopped agent:

```bash
docker start go-agent
docker attach go-agent
```

To go inside an agent (and execute inner Docker commands):

```bash
docker exec -it go-agent bash
docker  --host=tcp://localhost:1234 ps -a #for example
```

