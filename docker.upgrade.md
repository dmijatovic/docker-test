# Docker Engine on Linux Mint 18 (Ubuntu 16 - xenial)

Steps to upgrade docker engine.
BTW: use same steps to install :-).

```bash
  # remove old version
  sudo apt-get remove docker docker-engine docker.io

  # update system
  sudo apt-get update

  # Install packages to allow apt to use a repository over HTTPS:
  sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common


  # Add Docker’s official GPG key
  curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

  # Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.
  sudo apt-key fingerprint 0EBFCD88

  # Use the following command to set up the stable repository in a distribution like Linux Mint, you might need to change $(lsb_release -cs) to your parent Ubuntu distribution. For example, if you are using Linux Mint 18, you could use xenial (see below).

  sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   xenial \
   stable"

  # update apt  
  sudo apt-get update

  # install latest version of docker engine (ce)
  sudo apt-get install docker-ce

```
