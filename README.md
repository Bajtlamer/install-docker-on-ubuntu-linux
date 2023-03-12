## Install Docker on Ubuntu linux
This is a small guide, how to install docker on Ubuntu linux

## Prepare system for the installation
```console
apt-get update

sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```
#### Add Docker’s official GPG key:
```console
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
```
#### Use following command to setup repository
```console
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
## Install Docker Engine
```console
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
List the versions available in your repo:
```console
apt-cache madison docker-ce
```
Install a specific version using the version string from the second column, for example, `5:20.10.16~3-0~ubuntu-jammy`.
```console
sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io docker-compose-plugin
```
Now, run the first docker container:
```console
sudo docker run hello-world
```
## Post-installation steps for Linux
Create the `docker` group.
```console
sudo groupadd docker
```
Add your user to the docker group:
```console
sudo usermod -aG docker $USER

newgrp docker 
```
Verify, that you can run `docker` commands without `sudo`:
```console
docker run hello-world
```

## Possible issues
If zou running docker wit a error:

```console
WARNING: Error loading config file: /home/user/.docker/config.json -
stat /home/user/.docker/config.json: permission denied
```
then try to fix this problem, either remove the ~/.docker/ directory (it is recreated automatically, but any custom settings are lost), or change its ownership and permissions using the following commands:
```console
sudo chown "$USER":"$USER" /home/"$USER"/.docker -R
sudo chmod g+rwx "$HOME/.docker" -R
 ```
 
## Configure Docker to start on boot
```console
sudo systemctl enable docker.service
sudo systemctl enable containerd.service
```
To disable this behavior, use disable instead.
```console
sudo systemctl disable docker.service
sudo systemctl disable containerd.service
```
## Install Docker-Compose
```console
url -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

docker-compose --version
```
#### Test installation
Create a new directory for your sample container
```console
mkdir test
```
Change directory that you just created:
```console
vim docker-compose.yaml
```
And copy the following configuration into `docker-compose.yaml` file:
```console
version: '3.3'
services:
   hello-world:
      image:
         hello-world:latest
```
Next, run the following command to pull the hello-world image on your system:
```console
docker-composer up
```

