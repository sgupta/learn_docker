# First command to test your Docker installation. This will fetch a small imgae which print "Hello World"

 `docker run hello-world`
 
        [sgupta3@dockermgr1 learn_dcoker]$ sudo docker run hello-world
        Hello from Docker!
        This message shows that your installation appears to be working correctly.

        To generate this message, Docker took the following steps:
         1. The Docker client contacted the Docker daemon.
         2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
            (amd64)
         3. The Docker daemon created a new container from that image which runs the
            executable that produces the output you are currently reading.
         4. The Docker daemon streamed that output to the Docker client, which sent it
            to your terminal.

        To try something more ambitious, you can run an Ubuntu container with:
         $ docker run -it ubuntu bash

        Share images, automate workflows, and more with a free Docker ID:
         https://hub.docker.com/

        For more examples and ideas, visit:
         https://docs.docker.com/get-started/

        [sgupta3@dockermgr1 learn_dcoker]$
# Pull image from docker hub or other repositiry 
`docker pull <image>`

`docker pull centos:centos7`

      '[sgupta3@dockermgr1 learn_dcoker]$ sudo docker pull sgupta/ubuntu_example
      Using default tag: latest
      Trying to pull repository docker.io/sgupta/ubuntu_example ...
      latest: Pulling from docker.io/sgupta/ubuntu_example
      2de59b831a23: Pull complete
      354c3661655e: Pull complete
      91930878a2d7: Pull complete
      a3ed95caeb02: Pull complete
      8d394a2a67ef: Pull complete
      95ca3a90f576: Pull complete
      Digest: sha256:800b1c2c05be18b30e907dcd142d526ab33cb8e364fd4735edaa204d5186dfab
      Status: Downloaded newer image for docker.io/sgupta/ubuntu_example:latest
      [sgupta3@dockermgr1 learn_dcoker]$`
