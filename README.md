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
 [sgupta3@dockermgr2 example2]$ sudo docker build . -t example3 -f ./dockerfile
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

$ docker build -t sgupta3/myapp .

```
## Dockerfile format
Docker runs instructions in a Dockerfile in order.
### FROM
A Dockerfile must begin with a `FROM` instruction. FROM instructions support variables that are declared by any ARG instructions that occur before the first FROM.Optionally a name can be given to a new build stage by adding AS name to the FROM instruction. The name can be used in subsequent FROM and COPY --from=<name|index> instructions to refer to the image built in this stage.

FROM [--platform=<platform>] <image>[:<tag>] [AS <name>]
 
Example 1 - One mandotory instructiom if FROM
```
[sgupta3@dockermgr2 example1]$ cat dockerfile
#example 1 simple FROM
FROM busybox
[sgupta3@dockermgr2 example1]$ sudo docker build . -f ./dockerfile
Sending build context to Docker daemon 2.048 kB
Step 1/1 : FROM busybox
 ---> 83aa35aa1c79
Successfully built 83aa35aa1c79
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
