Build Notes
====
Original rticle found here [https://www.docker.com/blog/getting-started-with-docker-for-arm-on-linux/](https://www.docker.com/blog/getting-started-with-docker-for-arm-on-linux/)


Load/Install qemu
----
*This loads the qemu image from ARM on x86 systems. Probably should get the latest image from here [https://hub.docker.com/r/docker/binfmt/tags?page=1&ordering=last_updated](https://hub.docker.com/r/docker/binfmt/tags?page=1&ordering=last_updated)*

```
$ docker run --rm --privileged docker/binfmt:a7996909642ee92942dcd6cff44b9b95f08dad64
``` 


Check to see that qemu is enabled
---
```
$ cat /proc/sys/fs/binfmt_misc/qemu-aarch64
enabled
interpreter /usr/bin/qemu-aarch64
flags: OCF
offset 0
magic 7f454c460201010000000000000000000200b7
```


Create the Builder
-----
*I called it armbuilder but thats not really accurate since it can build on many architectures*
```
$ docker buildx create --name armbuilder
$ docker buildx use armbuilder
$ docker buildx inspect --bootstrap
Name:   armbuilder
Driver: docker-container

Nodes:
Name:      armbuilder0
Endpoint:  unix:///var/run/docker.sock
Status:    running
Platforms: linux/amd64, linux/arm64, linux/riscv64, linux/ppc64le, linux/s390x, linux/386, linux/arm/v7, linux/arm/v6
```

Go ahead and build (this is slow)
```
$ docker buildx build --platform linux/arm,linux/arm64 -t brabidou/docker-dante-pi:latest . --push 
```