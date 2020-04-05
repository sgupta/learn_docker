# learn_docker
Files created while learn docker
## 1
Build a hello word docker images from "busybee" linux contained using a shell script.This image will run and print "Hello World"
### Create Docker file 
//*
FROM busybox
COPY /print_hello.sh /
RUN sh /print_hello.sh
*//
##Shell script
//*
#!/bin/sh
echo "Hello World!"
*//
