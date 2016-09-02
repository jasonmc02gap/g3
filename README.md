# g3

to setup the environment just run:
```
sudo sh install.sh
```

regards

# Command sections explanation

## docker

the first section installs Docker app and the require software, libraries and repos that it needs

```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates
sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
sudo rm /etc/apt/sources.list.d/docker.list
sudo echo 'deb https://apt.dockerproject.org/repo ubuntu-trusty main' > /etc/apt/sources.list.d/docker.list
sudo apt-get update
sudo apt-get purge lxc-docker
sudo apt-get install init-system-helpers
sudo apt-cache policy docker-engine
sudo apt-get install linux-image-extra-$(uname -r) linux-image-extra-virtual
sudo apt-get install docker-engine
sudo service docker start
```

## docker-compose

the second section download and gives permissions to docker-compose CLI

```
sudo curl -L https://github.com/docker/compose/releases/download/1.7.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

## k8s

the last section creates a environment variable with the kubernetes version and the arquitecture type, then it starts a docker container with kubernetes software and then it download and gives permissions to kubectl CLI

```
export K8S_VERSION=$(curl -sS https://storage.googleapis.com/kubernetes-release/release/stable.txt)
export ARCH=amd64

docker run -d \
    --volume=/:/rootfs:ro \
    --volume=/sys:/sys:ro \
    --volume=/var/lib/docker/:/var/lib/docker:rw \
    --volume=/var/lib/kubelet/:/var/lib/kubelet:rw \
    --volume=/var/run:/var/run:rw \
    --net=host \
    --pid=host \
    --privileged \
    gcr.io/google_containers/hyperkube-${ARCH}:${K8S_VERSION} \
    /hyperkube kubelet \
        --containerized \
        --hostname-override=127.0.0.1 \
        --api-servers=http://localhost:8080 \
        --config=/etc/kubernetes/manifests \
        --allow-privileged --v=2

sudo curl -sSL "http://storage.googleapis.com/kubernetes-release/release/v1.2.0/bin/linux/amd64/kubectl" > /usr/bin/kubectl
sudo chmod +x /usr/bin/kubectl
```

# troubleshooting

## Docker: The following packages have unmet dependencies:...

you probably are not using ubuntu-trusty distro (14.04), change the line 10 with the ubuntu distro name that you are using

##  docker: Cannot connect to the Docker daemon. Is 'docker -d' running on this host?

run any docker command with sudo, or create a users-group and add docker user to it

