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

## The `sudo docker ps -a` command is used to list all Docker containers, including those that are currently running and those that have exited.
```shell
sudo docker ps -a
```
 Here's what each part of the command does:

- `sudo`: It is used to run the `docker` command with elevated privileges. The `docker` command often requires root or superuser permissions to interact with the Docker daemon.

- `docker`: This is the Docker command-line tool used for various Docker-related operations.

- `ps`: This subcommand is short for "process status" and is used to list Docker containers.

- `-a`: This option is used to list all containers, including both running and exited ones. If you omit the `-a` flag, only the running containers will be displayed.

When you run `sudo docker ps -a`, you will see a list of all Docker containers, with information such as their container ID, image name, command that was used to start the container, status, ports, and more. This information is useful for monitoring and managing Docker containers on your system.
