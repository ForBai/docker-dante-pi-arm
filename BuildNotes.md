Build Notes
====
Details found here [https://www.docker.com/blog/getting-started-with-docker-for-arm-on-linux/](https://www.docker.com/blog/getting-started-with-docker-for-arm-on-linux/)


This loads the qemu image form ARM on x86 systems. Probably should get the latest image from here [https://hub.docker.com/r/docker/binfmt/tags?page=1&ordering=last_updated](https://hub.docker.com/r/docker/binfmt/tags?page=1&ordering=last_updated)

```
$ docker run --rm --privileged docker/binfmt:a7996909642ee92942dcd6cff44b9b95f08dad64
``` 


Check to see that qemu is enabled
```
$ cat /proc/sys/fs/binfmt_misc/qemu-aarch64
enabled
interpreter /usr/bin/qemu-aarch64
flags: OCF
offset 0
magic 7f454c460201010000000000000000000200b7
```


Create the Builder
```
$ docker buildx create --name mybuilder
$ docker buildx use mybuilder
$ docker buildx inspect --bootstrap
Name: mybuilder
Driver: docker-container

Nodes:
Name: mybuilder0
Endpoint: unix:///var/run/docker.sock
Status: running
Platforms: linux/amd64, linux/arm64, linux/arm/v7, linux/arm/v6
```

Go ahead and build (this is slow)
```
$ docker buildx build --platform linux/arm,linux/arm64,linux/amd64 -t brabidou/docker-dante-pi:latest . --push 
```