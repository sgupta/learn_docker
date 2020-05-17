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
Docker runs instructions in a Dockerfile in order. A Dockerfile must begin with a `FROM` instruction.
Docker treats lines that begin with # as a comment
Example 1
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
