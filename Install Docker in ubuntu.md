1. After install ubuntu, if you want use docker, run command `sudo apt install docker.io`
2. check version docker using command `docker version` will display as below
    ```
    Client:
     Version:           20.10.25
     API version:       1.41
     Go version:        go1.18.1
     Git commit:        20.10.25-0ubuntu1~20.04.1
     Built:             Fri Jul 14 22:00:45 2023
     OS/Arch:           linux/amd64
     Context:           default
     Experimental:      true
    Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied
    ```
    after you check version docker and found error like below
    ```
    Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/version": dial unix /var/run/docker.sock: connect: permission denied
    ```
    run using command
    > 1. `sudo usermod -aG docker $USER`
    > 2. remove file docker.sock using comand `sudo rm /var/run/docker.sock`
    > 3. restart service docker using command `sudo systemctl restart docker`
    > 4. check version again and make sure there is no error like before using command `docker version`
    and will display like below
    ```
    Client:
     Version:           20.10.25
     API version:       1.41
     Go version:        go1.18.1
     Git commit:        20.10.25-0ubuntu1~20.04.1
     Built:             Fri Jul 14 22:00:45 2023
     OS/Arch:           linux/amd64
     Context:           default
     Experimental:      true
    
    Server:
     Engine:
      Version:          20.10.25
      API version:      1.41 (minimum version 1.12)
      Go version:       go1.18.1
      Git commit:       20.10.25-0ubuntu1~20.04.1
      Built:            Thu Jun 29 21:55:06 2023
      OS/Arch:          linux/amd64
      Experimental:     false
     containerd:
      Version:          1.7.2
      GitCommit:        
     runc:
      Version:          1.1.7-0ubuntu1~20.04.1
      GitCommit:        
     docker-init:
      Version:          0.19.0
      GitCommit:   
    ```
3. in this process i wanna pull image that i want to use
    > 1. Pull [ubuntu image(focal) ](https://hub.docker.com/_/ubuntu/tags) using command `docker pull ubuntu:focal`
