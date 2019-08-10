# devopsdocker2019

   * [Part 1 exercises](#part-1-exercises)
      * [Exercise 1.1](#exercise-11)
      * [Exercise 1.2](#exercise-12)
      * [Exercise 1.3](#exercise-13)
      * [Exercise 1.4](#exercise-14)
      * [Exercise 1.5](#exercise-15)
      * [Exercise 1.6](#exercise-16)
      * [Exercise 1.7](#exercise-17)
      * [Exercise 1.8](#exercise-18)
      * [Exercise 1.9](#exercise-19)
      * [Exercise 1.10](#exercise-110)
      * [Exercise 1.11](#exercise-111)
      * [Exercise 1.12](#exercise-112)
      * [Exercise 1.13](#exercise-113)
      * [Exercise 1.14](#exercise-114)

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
RUN apt-get install -y git
RUN git clone https://github.com/docker-hy/frontend-example-docker.git .
EXPOSE 5000
RUN npm install
CMD ["npm", "start"]
```
On host machine:

`$ docker build -t frontend .`

`$ docker run -p 5000:5000 frontend`

On browser when connecting to localhost:5000:
```
Exercise 1.10: Congratulations! You configured your ports correctly!
```

### Exercise 1.11
Dockerfile
```
FROM ubuntu:latest
WORKDIR /usr/src/app
RUN apt-get update
RUN apt-get -y install curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs
RUN apt-get install -y git
RUN git clone https://github.com/docker-hy/backend-example-docker.git .
EXPOSE 8000
RUN npm install
CMD ["npm", "start"]
```
On host machine:

`$ docker build -t backend .`

`$ docker run -v $(pwd)/logs.txt:/usr/src/app/logs.txt -p 8000:8000 backend`

On browser when connecting to localhost:8000:
```
Port configured correctly, generated message in logs.txt
```
logs.txt
```
8/9/2019, 5:04:28 PM: Connection received in root
8/9/2019, 5:04:28 PM: Connection received in root
8/9/2019, 5:04:29 PM: Connection received in root
```

### Exercise 1.12
backend Dockerfile
```
FROM ubuntu:latest
WORKDIR /usr/src/app
RUN apt-get update
RUN apt-get -y install curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs
RUN apt-get install -y git
RUN git clone https://github.com/docker-hy/backend-example-docker.git .
EXPOSE 8000
ENV FRONT_URL="http://localhost:5000"
RUN npm install
CMD npm start
```

frontend Dockerfile
```
FROM ubuntu:latest
WORKDIR /usr/src/app
RUN apt-get update
RUN apt-get -y install curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs
RUN apt-get install -y git
RUN git clone https://github.com/docker-hy/frontend-example-docker.git .
EXPOSE 5000
ENV API_URL="http://localhost:8000"
RUN npm install
CMD npm start
```

start commands

`$ docker run -it --rm -v $(pwd)/logs.txt:/usr/src/app/logs.txt -p 8000:8000 backend`

`$ docker run -it --rm -p 5000:5000 frontend`

### Exercise 1.13
Dockerfile
```
FROM openjdk:8
RUN apt-get update
RUN apt-get install git
RUN git clone https://github.com/docker-hy/spring-example-project.git
WORKDIR spring-example-project/
EXPOSE 8080
RUN ./mvnw package
CMD java -jar ./target/docker-example-1.1.3.jar
```
`$ docker run -it --rm -p 8080:8080 spring`

### Exercise 1.14
Dockerfile
```
FROM ruby:2.6.0
WORKDIR /usr/src/app
RUN apt-get update
RUN apt-get -y install curl
RUN curl -sL https://deb.nodesource.com/setup_10.x | bash
RUN apt-get install -y nodejs
RUN apt-get install -y git
EXPOSE 3000
RUN git clone https://github.com/docker-hy/rails-example-project.git .
RUN gem install bundler
RUN bundle install
RUN rails db:migrate
CMD rails s
```
`$ docker run -it --rm -p 3000:3000 ruby`
