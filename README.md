# Learn Docker
  # Building a docker image
   Build A simple image with shellscript 
   Build a hello word docker images from "busybee" linux contained using a shell script.This image will run and print "Hello World"
   Create Dokerfile and a shell script as below in same directory 
 ```               
        #Dockerfile
        FROM busybox
        COPY /print_hello.sh /
        RUN sh /print_hello.sh
        
        #Shell script
        #!/bin/sh
        echo "Hello World!"
 
        Usage:  docker build [OPTIONS] PATH | URL | -

Build an image from a Dockerfile

Options:
      --build-arg list             Set build-time variables (default [])
  -f, --file string                Name of the Dockerfile (Default is 'PATH/Dockerfile')
      --force-rm                   Always remove intermediate containers
  -m, --memory string              Memory limit
      --memory-swap string         Swap limit equal to memory plus swap: '-1' to enable unlimited swap
      --network string             Set the networking mode for the RUN instructions during build (default "default")
      --no-cache                   Do not use cache when building the image
      --pull                       Always attempt to pull a newer version of the image
  -q, --quiet                      Suppress the build output and print image ID on success
      --rm                         Remove intermediate containers after a successful build (default true)
  -t, --tag list                   Name and optionally a tag in the 'name:tag' format (default [])
      --ulimit ulimit              Ulimit options (default [])
  -v, --volume list                Set build-time bind mounts (default [])

  Example0- 
 [sgupta3@dockermgr2 example2]$ sudo docker build . -t example3 -f ./dockerfile --no-cache
 ```
# Docker file options
Docker can build images automatically by reading the instructions from a Dockerfile. A Dockerfile is a text document that contains all the commands a user could call on the command line to assemble an image. Using docker build users can create an automated build that executes several command-line instructions in succession.
The docker build command builds an image from a Dockerfile and a context. The build’s context is the set of files at a specified location PATH or URL. The PATH is a directory on your local filesystem. The URL is a Git repository location.

A context is processed recursively. So, a PATH includes any subdirectories and the URL includes the repository and its submodules. This example shows a build command that uses the current directory as context:
```
$ docker build .

#Traditionally, the Dockerfile is called Dockerfile and located in the root of the context. You use the -f flag with docker build to point to a Dockerfile anywhere in your file system.

$ docker build -f /path/to/a/Dockerfile .

#You can specify a repository and tag at which to save the new image if the build succeeds:

$ docker build -t sgupta/myapp .

```
OPTION | FUNCTION | EXTRAS
------------ | -------------|-----------
FROM | Download base image for build | "FROM centos7:latest"
MAINTAINER | User to set name of person or company maintaining image | `MAINTAINER sgupta`
LABEL | Just add metadata to your image in key=value format | `LABEL os=centos, date=20200517`
ENV | Environment varibales | "ENV HOME=/home/sgupta"
WORKDIR |Set working directory inside image being prossed| "WORKDIR ${HOME}/project1"
COPY | Copy files or directoreis from source(local space) to destination(inside image)| `COPY python/*.py .`
ADD | Same as copy but can **use url as source and extract a tar file in destination**|`ADD https://getenvoy.io/cli project1/`
RUN | Execute commands during build | `RUN cd /root` or `RUN ["ls", "-l", "/root"]`
CMD | Either an executable or list of arguments to be passed when container run | `CMD ["httpd", "-f", "/etc/httpd.conf"] or CMD ["--logdir", "/var/logs"]`
ENTRYPOINT | Conatiner on will will execute the execuatble and pass any arguments from CMD | `ENTRYPOINT ["startup.sh", "--debug"]`
EXPOSE | Set ports to listen on runtime |`EXPOSE 8080`
VOLUME | create a mount point inside image which on docker run will be available to mount on local file-system| `VOLUMME ["/data"] 


## Dockerfile format
Docker runs instructions in a Dockerfile in order.
### FROM
A Dockerfile must begin with a `FROM` instruction. FROM instructions support variables that are declared by any ARG instructions that occur before the first FROM.Optionally a name can be given to a new build stage by adding AS name to the FROM instruction. The name can be used in subsequent FROM and COPY --from=<name|index> instructions to refer to the image built in this stage.
With multi-stage builds, you use multiple FROM statements in your Dockerfile. Each FROM instruction can use a different base, and each of them begins a new stage of the build. You can selectively copy artifacts from one stage to another, leaving behind everything you don’t want in the final image.


```FROM [--platform=<platform>] <image>[:<tag>] [AS <name>] ```

```
# Example 1.a - Simple single image build using FROM
[sgupta3@dockermgr2 example1]$ cat dockerfile
#example 1 simple FROM
FROM busybox
[sgupta3@dockermgr2 example1]$ sudo docker build . -f ./dockerfile
Sending build context to Docker daemon 2.048 kB
Step 1/1 : FROM busybox
 ---> 83aa35aa1c79
Successfully built 83aa35aa1c79
[sgupta3@dockermgr2 example1]$

Example 1.b - Multistage build using AS statement with image and copy file from one image to other
[sgupta3@dockermgr2 example1]$ cat dockerfile
#example 1.b mutli image with AS and copy file from one image to other
FROM busybox:glibc AS image1
ENV  WORKDIR=/home/sgupta
WORKDIR ${WORKDIR}
RUN ["touch", "busybox.txt"]
RUN ["echo", "from busybox image", ">", "busybox.txt"]
RUN ["cat", "busybox.txt"]

FROM alpine:latest
WORKDIR ${WORKDIR}/alpine
COPY --from=image1 /home/sgupta/busybox.txt .
RUN ls
RUN ["cat", "busybox.txt"]
[sgupta3@dockermgr2 example1]$
[sgupta3@dockermgr2 example1]$ sudo docker build . -t example4 -f ./dockerfile --no-cache
Sending build context to Docker daemon  2.048kB
Step 1/11 : FROM busybox:glibc AS image1
 ---> 845454170a51
Step 2/11 : ENV  WORKDIR=/home/sgupta
 ---> Running in 0255d54b7bdd
Removing intermediate container 0255d54b7bdd
 ---> 8f43c62be8ed
Step 3/11 : WORKDIR ${WORKDIR}
 ---> Running in 00e81b3414e8
Removing intermediate container 00e81b3414e8
 ---> 076a15e73904
Step 4/11 : RUN ["touch", "busybox.txt"]
 ---> Running in f5410e9b4a90
Removing intermediate container f5410e9b4a90
 ---> 01d81a26fe60
Step 5/11 : RUN ["echo", "from busybox image", ">", "busybox.txt"]
 ---> Running in 0dde17aa3a73
from busybox image > busybox.txt
Removing intermediate container 0dde17aa3a73
 ---> 5f9c987be75b
Step 6/11 : RUN ["cat", "busybox.txt"]
 ---> Running in 8476d5d76dd3
Removing intermediate container 8476d5d76dd3
 ---> 014dc8e4f055
Step 7/11 : FROM alpine:latest
 ---> f70734b6a266
Step 8/11 : WORKDIR ${WORKDIR}/alpine
 ---> Running in ee896125dc30
Removing intermediate container ee896125dc30
 ---> bd5ad19e973d
Step 9/11 : COPY --from=image1 /home/sgupta/busybox.txt .
 ---> 7e602aea46b1
Step 10/11 : RUN ls
 ---> Running in fdf2af3296b3
busybox.txt
Removing intermediate container fdf2af3296b3
 ---> 82a9dbe46d50
Step 11/11 : RUN ["cat", "busybox.txt"]
 ---> Running in db9ba70b7836
Removing intermediate container db9ba70b7836
 ---> c923312ce01b
Successfully built c923312ce01b
Successfully tagged example4:latest
[sgupta3@dockermgr2 example1]$
```
### Environment variable
Environment variables (declared with the ENV statement) can also be used in certain instructions as variables to be interpreted by the Dockerfile. 
Environment variables are notated in the Dockerfile either with $variable_name or ${variable_name}. 
The ${variable_name} syntax also supports a few of the standard bash modifiers as specified below:

${variable:-word} indicates that if variable is set then the result will be that value. If variable is not set then word will be the result.
${variable:+word} indicates that if variable is set then word will be the result, otherwise the result is the empty string.
In all cases, word can be any string, including additional environment variables.

Escaping is possible by adding a \ before the variable: \$foo or \${foo}, for example, will translate to $foo and ${foo} literals respectively.

Environment variables are supported by the following list of instructions in the Dockerfile:

ADD
COPY
ENV
EXPOSE
FROM
LABEL
STOPSIGNAL
USER
VOLUME
WORKDIR
ONBUILD (when combined with one of the supported instructions above)
Example -
```
FROM busybox
ENV HOME="/home/sgupta"
WORKDIR ${HOME}
```
### WORKDIR
The WORKDIR instruction sets the working directory for any RUN, CMD, ENTRYPOINT, COPY and ADD instructions that follow it in the Dockerfile. If the WORKDIR doesn’t exist, it will be created even if it’s not used in any subsequent Dockerfile instruction.
The WORKDIR instruction can be used multiple times in a Dockerfile. If a relative path is provided, it will be relative to the path of the previous WORKDIR instruction. For example:
EXAMPLE
```
[sgupta3@dockermgr2 example2]$ cat dockerfile
FROM busybox:glibc
#Set environmenent variable HOME
ENV  HOME=/home/sgupta
#WORKDIR will change working directory inside image to WORKDIR
WORKDIR ${HOME}
#Too bad you can not use environment variable in RUN command
RUN ["touch", "/home/sgupta/key" ]
#This will prove working directory changed to $HOME
RUN pwd
#Again change working directory which will be relative to previously set
WORKDIR project
#Prove it
RUN pwd
[sgupta3@dockermgr2 example2]$
[sgupta3@dockermgr2 example2]$ sudo docker build . -t example3 -f ./dockerfile
Sending build context to Docker daemon 2.048 kB
Step 1/7 : FROM busybox:glibc
 ---> 845454170a51
Step 2/7 : ENV HOME /home/sgupta
 ---> Running in 195b9c9750db
 ---> 4e05e220ca87
Removing intermediate container 195b9c9750db
Step 3/7 : WORKDIR ${HOME}
 ---> fdf106486056
Removing intermediate container 42a59100972a
Step 4/7 : RUN touch /home/sgupta/key
 ---> Running in 9874f2f0bd54

 ---> c2efe72c0a57
Removing intermediate container 9874f2f0bd54
Step 5/7 : RUN pwd
 ---> Running in 9eb2b678c3f8

/home/sgupta
 ---> 1c9cd484ab4d
Removing intermediate container 9eb2b678c3f8
Step 6/7 : WORKDIR project
 ---> d1688aceeabb
Removing intermediate container 419fc767606b
Step 7/7 : RUN pwd
 ---> Running in 8de31e0e6daf

/home/sgupta/project
 ---> a8b338117ef1
Removing intermediate container 8de31e0e6daf
Successfully built a8b338117ef1
[sgupta3@dockermgr2 example2]$ 
```

### RUN 
The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.Layering RUN instructions and generating commits conforms to the core concepts of Docker where commits are cheap and containers can be created from any point in an image’s history, much like source control.
RUN has 2 forms:

1. RUN <command> (shell form, the command is run in a shell, which by default is /bin/sh -c

2. RUN ["executable", "param1", "param2"] (exec form) ##The exec form makes it possible to avoid shell string munging, and to RUN commands using a base image that does not contain the specified shell executable.

example 2

```
[sgupta3@dockermgr2 example2]$ cat dockerfile
FROM busybox:glibc
RUN /bin/sh -c 'echo TEST1;echo TEST2'
RUN ["/bin/sh", "-c", "echo TEST3"]
RUN ls

[sgupta3@dockermgr2 example2]$ sudo docker build . -f ./dockerfile
Sending build context to Docker daemon 2.048 kB
Step 1/4 : FROM busybox:glibc
 ---> 845454170a51
Step 2/4 : RUN /bin/sh -c 'echo TEST1;echo TEST2'
 ---> Running in 7c9315962f8e

TEST1
TEST2
 ---> 19478c6f0387
Removing intermediate container 7c9315962f8e
Step 3/4 : RUN /bin/sh -c echo TEST3
 ---> Running in 1387f9f148a3

TEST3
 ---> c85209aae09f
Removing intermediate container 1387f9f148a3
Step 4/4 : RUN ls
 ---> Running in 0ea265775c0c

bin
dev
etc
 ---> 574c88db8639
Removing intermediate container 0ea265775c0c
Successfully built 574c88db8639
[sgupta3@dockermgr2 example2]$
```
### CMD
  There can only be one CMD instruction in a Dockerfile. If you list more than one CMD then only the last CMD will take effect.
  The main purpose of a CMD is to provide defaults for an executing container. These defaults can include an executable, or they can omit the executable, in which case you must specify an ENTRYPOINT instruction as well.
  Any aruguments in CMD field for executable can be override from command line docker run or docker compose file.
  If CMD is used to provide default arguments for the ENTRYPOINT instruction, both the CMD and ENTRYPOINT instructions should be specified with the JSON array format.The CMD instruction has three forms:

    CMD ["executable","param1","param2"] (exec form, this is the preferred form)
    CMD ["param1","param2"] (as default parameters to ENTRYPOINT)
    CMD command param1 param2 (shell form)

```
#Example 1 with CMD executable with arguments passed 
[sgupta3@dockermgr2 example1]$ cat dockerfile
FROM alpine:latest
WORKDIR /home/sgupta
#CMD will do ls command on /bin
CMD ["ls", "/bin"]
[sgupta3@dockermgr2 example1]$
[sgupta3@dockermgr2 example1]$ sudo docker build . -f ./dockerfile -t example6
Sending build context to Docker daemon  75.14MB
Step 1/3 : FROM alpine:latest
 ---> f70734b6a266
Step 2/3 : WORKDIR /home/sgupta
 ---> Using cache
 ---> 41144e9936a5
Step 3/3 : CMD ["ls", "/bin"]
 ---> Using cache
 ---> cf144e0a0ff3
Successfully built cf144e0a0ff3
Successfully tagged example6:latest
[sgupta3@dockermgr2 example1]$ sudo docker run -it example6
arch           echo           kill           netstat        sh
ash            ed             link           nice           sleep
base64         egrep          linux32        pidof          stat
bbconfig       false          linux64        ping           stty
busybox        fatattr        ln             ping6          su
cat            fdflush        login          pipe_progress  sync
chgrp          fgrep          ls             printenv       tar
chmod          fsync          lzop           ps             touch
chown          getopt         makemime       pwd            true
conspy         grep           mkdir          reformime      umount
cp             gunzip         mknod          rev            uname
date           gzip           mktemp         rm             usleep
dd             hostname       more           rmdir          watch
df             ionice         mount          run-parts      zcat
dmesg          iostat         mountpoint     sed
dnsdomainname  ipcalc         mpstat         setpriv
dumpkmap       kbd_mode       mv             setserial
[sgupta3@dockermgr2 example1]$
[sgupta3@dockermgr2 example1]$ sudo docker run -it example6 "ls" "/etc"
alpine-release  inittab         opt             services
apk             issue           os-release      shadow
conf.d          logrotate.d     passwd          shells
crontabs        modprobe.d      periodic        ssl
fstab           modules         profile         sysctl.conf
group           modules-load.d  profile.d       sysctl.d
hostname        motd            protocols       udhcpd.conf
hosts           mtab            resolv.conf
init.d          network         securetty
[sgupta3@dockermgr2 example1]$
```

### ENTRYPOINT
An ENTRYPOINT allows you to configure a container that will run as an executable.Command line arguments to docker run <image> will be appended after all elements in an exec form ENTRYPOINT, and will override all elements specified using CMD. This allows arguments to be passed to the entry point, i.e., docker run <image> -d will pass the -d argument to the entry point. You can override the ENTRYPOINT instruction using the docker run --entrypoint flag.
 
 ```
FROM ubuntu
ENTRYPOINT ["top", "-b"]
CMD ["-c"]

$ docker run -it --rm --name test  top -H

top - 08:25:00 up  7:27,  0 users,  load average: 0.00, 0.01, 0.05
Threads:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0.1 us,  0.1 sy,  0.0 ni, 99.7 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem:   2056668 total,  1616832 used,   439836 free,    99352 buffers
KiB Swap:  1441840 total,        0 used,  1441840 free.  1324440 cached Mem

  PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
    1 root      20   0   19744   2336   2080 R  0.0  0.1   0:00.04 top
 ```
### USER
### EXPOSE

### All inclusive example
```
[sgupta3@dockermgr2 example1]$ cat dockerfile
FROM alpine:latest
LABEL os=alpine, date=20200517
WORKDIR /home/sgupta
RUN mkdir project1
COPY copy/ project1/
COPY script/start.sh bin/
ADD https://getenvoy.io/cli project1/
RUN ls project1
VOLUME ["/home/sgupta/project1"]
ENTRYPOINT ["/bin/start.sh"]

```
