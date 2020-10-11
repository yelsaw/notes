
# Docker Community Edition (CE) Installation on Debian
Run commands as-is to install the latest version of `docker-ce` on Debian 10`buster`or Debian 9`stretch`
### Install prerequisites
Install the packages required to add GPG key and docker repository:
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common gnupg2 -y
```
### Import docker repository GPG key
```
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
```
The above command should return `OK` if successful.

### Add docker source to /etc/apt/source.list.d/docker.list
`$(lsb_release -cs)` should return `buster` or `stretch` contingent on your Debian version, anything else hasn't been tested. 
```
echo "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list
```
### Update sources and install docker-ce
Update apt and install `docker-ce` and `docker-ce-cli`
```
sudo apt update; sudo apt install docker-ce -y
```
### Verify installation
After `docker-ce` is installed it should automatically start the service, verify with the following command.
```
sudo systemctl status docker
```
If docker is running the above command should output something reminiscent to this:
```
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2020-10-11 12:06:22 CDT; 1min 33s ago
     Docs: https://docs.docker.com
 Main PID: 26681 (dockerd)
...
```
# Using docker without sudo
Docker needs to be run with elevated permissions by default but if you want to run without `sudo` follow the remaining steps.
### Add user to `docker` group
```
sudo usermod -aG docker $USER
```
### Activate new group for current user
Once added you can `switch user` in your current session to become part of the `docker`  group or logout and log back for changes to take affect globally.
```
su $USER
```
### Verify permissions
If you've been successfully added to the `docker` group and refreshed your user's groups, you should be able to run the de facto *docker* `hello-world` image using this command.

Docker will automatically pull and run the required images if they exist in public docker repository.
```
docker run hello-world
```
Running the command above should output something reminiscent to the following:
```
Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
...
```
