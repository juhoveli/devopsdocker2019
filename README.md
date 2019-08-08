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

`root@d40ba1864d9a:/usr/app# tail -f logs.txt `
```
...
Thu, 08 Aug 2019 18:43:45 GMT
Secret message is:
"Docker is easy"
...
```
