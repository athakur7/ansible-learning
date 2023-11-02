# Session 4 - Managed node on cloud & Configuring Docker


## Installing & configuring docker on ec2-instance RHEL/centOS

1. **Configure yum repo for Docker**
    ```shell
    sudo yum install -y yum-utils
    sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
    ```
2. **Docker Installation**
    ```shell
    sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
    ```
4. **Start Docker Service**
    ```shell
    sudo systemctl start docker
    ```
5. **Verify that the Docker Engine installation is successful by running the hello-world image.**
    ```shell
    sudo docker run hello-world
    ```
## Replacing characters in vi editor in below we are replacing /rhel characters with /centos
```shell
:%s /rhel/centos
```