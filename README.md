# Learn Docker
  # Building a docker image
   Build A simple image with shellscript 
   Build a hello word docker images from "busybee" linux contained using a shell script.This image will run and print "Hello World"
   Create Dokerfile and a shell script as below in same directory 
                
        #Dockerfile
        FROM busybox
        COPY /print_hello.sh /
        RUN sh /print_hello.sh
        
        #Shell script
        #!/bin/sh
        echo "Hello World!"
      
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

### RUN 
The RUN instruction will execute any commands in a new layer on top of the current image and commit the results. The resulting committed image will be used for the next step in the Dockerfile.Layering RUN instructions and generating commits conforms to the core concepts of Docker where commits are cheap and containers can be created from any point in an image’s history, much like source control.
RUN has 2 forms:

1. RUN <command> (shell form, the command is run in a shell, which by default is /bin/sh -c

2. RUN ["executable", "param1", "param2"] (exec form) ##The exec form makes it possible to avoid shell string munging, and to RUN commands using a base image that does not contain the specified shell executable.

example 2

```
[sgupta3@dockermgr2 example2]$ cat dockerfile
FROM busybox:glibc
RUN /bin/bash -c 'echo TEST1;echo TEST2'
RUN ["/bin/bash", "-c", "echo TEST3"]
RUN ls
[sgupta3@dockermgr2 example2]$ sudo docker build . -f ./dockerfile
Sending build context to Docker daemon 2.048 kB
Step 1/4 : FROM busybox:glibc
 ---> 845454170a51
Step 2/4 : RUN /bin/bash -c 'echo TEST1;echo TEST2'
 ---> Running in 7b6ae6801418

/bin/sh: /bin/bash: not found
The command '/bin/sh -c /bin/bash -c 'echo TEST1;echo TEST2'' returned a non-zero code: 127
[sgupta3@dockermgr2 example2]$ vim dockerfile
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
home
lib
lib64
proc
root
run
sys
tmp
usr
var
 ---> 574c88db8639
Removing intermediate container 0ea265775c0c
Successfully built 574c88db8639
[sgupta3@dockermgr2 example2]$
```
