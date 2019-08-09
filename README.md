# devopsdocker2019

## Part 1 exercises

### Exercise 1.1
`$ docker ps -a`
```
CONTAINER ID        IMAGE                 COMMAND                  CREATED             STATUS                     PORTS               NAMES
2671bb47f57a        nginx                 "nginx -g 'daemon of…"   28 seconds ago      Up 27 seconds              80/tcp              stoic_clarke
b9aa5c805fe6        nginx                 "nginx -g 'daemon of…"   48 seconds ago      Exited (0) 9 seconds ago                       hardcore_mayer
9fa3c25d2be5        nginx                 "nginx -g 'daemon of…"   51 seconds ago      Exited (0) 4 seconds ago                       angry_sutherland
```

### Exercise 1.2
`$ docker ps -a`
```
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
```

`$ docker images`
```
REPOSITORY            TAG                 IMAGE ID            CREATED             SIZE
```

### Exercise 1.3
`$ docker run -it devopsdockeruh/pull_exercise`
```
...
Give me the password: basics
You found the correct password. Secret message is:
"This is the secret message"
```

### Exercise 1.4
`$ docker run devopsdockeruh/exec_bash_exercise`

`$ docker start sad_mcclintock`

`$ docker exec -it sad_mcclintock bash`

`# tail -f logs.txt `
```
...
Thu, 08 Aug 2019 18:43:45 GMT
Secret message is:
"Docker is easy"
...
```

### Exercise 1.5
`$ docker run --rm -it ubuntu:16.04 sh -c 'apt-get update; apt-get install curl; echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;'`
```
...
Input website:
helsinki.fi
Searching..
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN">
<html><head>
<title>301 Moved Permanently</title>
</head><body>
<h1>Moved Permanently</h1>
<p>The document has moved <a href="http://www.helsinki.fi/">here</a>.</p>
</body></html>
```

### Exercise 1.6
Dockerfile
```
FROM devopsdockeruh/overwrite_cmd_exercise 
CMD ["-c"]
```
`$ docker build -t docker-clock .`

`$ docker run docker-clock`

### Exercise 1.7
Dockerfile 
```
FROM ubuntu:16.04
RUN apt-get update 
RUN apt-get -y install curl
COPY script.sh .
RUN chmod +x script.sh
CMD ./script.sh
```
script.sh
```
#!/bin/bash
echo "Input website:"; read website; echo "Searching.."; sleep 1; curl http://$website;
```
`$ docker build -t curler .`

`$ docker run -it --rm curler`

### Exercise 1.8

`$ touch logs.txt`

`$ docker run -v $(pwd)/logs.txt:/usr/app/logs.txt devopsdockeruh/first_volume_exercise`
```
(node:1) ExperimentalWarning: The fs.promises API is experimental
Wrote to file /usr/app/logs.txt
Wrote to file /usr/app/logs.txt
```
`$ cat logs.txt` (on host)
```
Fri, 09 Aug 2019 13:26:52 GMT
Fri, 09 Aug 2019 13:26:55 GMT
```

### Exercise 1.9
`$ docker run -p 5000:80 devopsdockeruh/ports_exercise`

### Exercise 1.10
Dockerfile
```
FROM ubuntu:latest
WORKDIR /usr/src/app
RUN apt-get update
RUN apt-get -y install curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs
COPY package.json package-lock.json* /usr/src/app/
EXPOSE 5000
RUN npm install
COPY . .
CMD ["npm", "start"]
```
On host machine:
`$ git clone https://github.com/docker-hy/frontend-example-docker.git`

`$ docker build -t frontend .`

`$ docker run -it -p 5000:5000 frontend`

Browser localhost:5000
```
Exercise 1.10: Congratulations! You configured your ports correctly!
```
