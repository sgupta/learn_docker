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
      
