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

# Other commands
To list existing images

`docker images`

To run a container either from local images or remote

`docker run <image>`

`docker run sgupta/ubuntu_example`

List current running containers 

`docker ps`

List all current and past running containers 

`docker ps -a`

Stop a running conatiner 

`docker stop <container id>`  - Note: use "docker ps" command to find container id

Run a container and connect to shell

`sudo docker run -it sgupta/ubuntu_example "/bin/bash"`

Execute command inside conatiner

`sudo docker exec <container id> "date"`

`sudo docker exec <container id> "/bin/bash"` Note - This can be used to connect after starting a contaier to connect

Providing a Name alias to a container to use instead of id

`dokcer run --name ubuntu sgupta/ubuntu_example`

Remove images not required

`docker rm <image id>` 

`docker rm $(docker ps -a -q -f status=exited)` note - Find and remove images from stopped container"`

`docker conatiner prune` Note - Remove all images from stopped conatiners

`docker run --rm  busybox` Note - This remove image after container stop

# Inspect conatiner for all metadata 

`docker inspect <conatiner id>`
 
 ```$ sudo docker inspect 58155893f7a8
[
    {
        "Id": "58155893f7a89824773657e411ddd308310db545de2df87954471da842337802",
        "Created": "2020-04-05T04:04:40.309716476Z",
        "Path": "nginx",
        "Args": [
            "-g",
            "daemon off;"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 6490,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-04-05T04:04:42.96304882Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:ed21b7a8aee9cc677df6d7f38a641fa0e3c05f65592c592c9f28c42b3dd89291",
        "ResolvConfPath": "/var/lib/docker/containers/58155893f7a89824773657e411ddd308310db545de2df87954471da842337802/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/58155893f7a89824773657e411ddd308310db545de2df87954471da842337802/hostname",
        "HostsPath": "/var/lib/docker/containers/58155893f7a89824773657e411ddd308310db545de2df87954471da842337802/hosts",
        "LogPath": "",
        "Name": "/cocky_banach",
        "RestartCount": 0,
        "Driver": "overlay2",
        "MountLabel": "system_u:object_r:svirt_sandbox_file_t:s0:c86,c610",
        "ProcessLabel": "system_u:system_r:svirt_lxc_net_t:s0:c86,c610",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "journald",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "docker-runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": null,
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DiskQuota": 0,
            "KernelMemory": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": -1,
            "OomKillDisable": false,
            "PidsLimit": 0,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0
        },
        "GraphDriver": {
            "Name": "overlay2",
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/6f83e2043fe9f6e615d12118ae96e3df78d949119d3a5c37ae098fffa28f3511-init/diff:/var/lib/docker/overlay2/b618f8f4f0ec27b8e4f13d4f021eaa152253ce35155566a48532e697fb644ebc/diff:/var/lib/docker/overlay2/6cc56bfe8bfdef4ec5b15e3739d305cd8fdc51a732f270a6e176eb267c20625a/diff:/var/lib/docker/overlay2/c8e5cb2329a2d809bff1bd1df75d27a06fbdba809224b4e4126947d5501c5f79/diff",
                "MergedDir": "/var/lib/docker/overlay2/6f83e2043fe9f6e615d12118ae96e3df78d949119d3a5c37ae098fffa28f3511/merged",
                "UpperDir": "/var/lib/docker/overlay2/6f83e2043fe9f6e615d12118ae96e3df78d949119d3a5c37ae098fffa28f3511/diff",
                "WorkDir": "/var/lib/docker/overlay2/6f83e2043fe9f6e615d12118ae96e3df78d949119d3a5c37ae098fffa28f3511/work"
            }
        },
        "Mounts": [],
        "Config": {
            "Hostname": "58155893f7a8",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "80/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "NGINX_VERSION=1.17.9",
                "NJS_VERSION=0.3.9",
                "PKG_RELEASE=1~buster"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "ArgsEscaped": true,
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGTERM"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "a74d5f17fa2ff090f964c5d9cf269d989bab23edaf8141a79b5f1924fec0e4d8",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {
                "80/tcp": null
            },
            "SandboxKey": "/var/run/docker/netns/a74d5f17fa2f",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "b172d86941a37890c299ae39bed788e1a15ee3d795c1e5f4f8a2bdb8fc73a22b",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "6c8c36dbef6560d194a55aa5bdbd778ebbde06b7b2307749300d1dd3f40c8424",
                    "EndpointID": "b172d86941a37890c299ae39bed788e1a15ee3d795c1e5f4f8a2bdb8fc73a22b",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02"
                }
            }
        }
    }
]
```
 To retrive specific section from inspect command used 
 ```
 sudo docker inspect 58155893f7a8 --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}'
172.17.0.2
```
```
sudo docker inspect 58155893f7a8 --format='{{json .Config}}'
{"Hostname":"58155893f7a8","Domainname":"","User":"","AttachStdin":false,"AttachStdout":false,"AttachStderr":false,"ExposedPorts":{"80/tcp":{}},"Tty":false,"OpenStdin":false,"StdinOnce":false,"Env":["PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin","NGINX_VERSION=1.17.9","NJS_VERSION=0.3.9","PKG_RELEASE=1~buster"],"Cmd":["nginx","-g","daemon off;"],"ArgsEscaped":true,"Image":"nginx","Volumes":null,"WorkingDir":"","Entrypoint":null,"OnBuild":null,"Labels":{"maintainer":"NGINX Docker Maintainers \u003cdocker-maint@nginx.com\u003e"},"StopSignal":"SIGTERM"}
````
 
 
# Run container as webserver and publish some Static contents and detach
Below command with start nginx container on Random host ports (-P) and detach from it (-d) to return prompt
```
sudo docker run -d -P nginx

```

Below command will start nginx container with host port mapped to container ports (-p port:port)
```
sudo docker run --name mynginx -d -p 80:80 nginx

sudo docker port myngixn
```

 Next file to read is mywebapp.MD 
