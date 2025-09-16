# Install docker



## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Supported Operating Systems](#supported-operating-systems)
3. [Installation](#installation)
4. [Configuration](#configuration)
5. [Service Management](#service-management)
6. [Troubleshooting](#troubleshooting)
7. [Security Considerations](#security-considerations)
8. [Performance Tuning](#performance-tuning)
9. [Backup and Restore](#backup-and-restore)
10. [System Requirements](#system-requirements)
11. [Support](#support)
12. [Contributing](#contributing)
13. [License](#license)
14. [Acknowledgments](#acknowledgments)
15. [Version History](#version-history)
16. [Appendices](#appendices)

### ZFS

```shell
zfs create -o mountpoint=/var/lib/docker rpool/docker
mkdir /etc/systemd/system/docker.service.d
curl -q -LSs https://github.com/casjay-base/howtos/raw/main/proxmox/storage-driver.conf >/etc/systemd/system/docker.service.d/storage-driver.conf
```

### Docker

```shell
apt-get install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common
curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
apt-key fingerprint 0EBFCD88
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
apt-get update && apt-get install docker-ce -y
```

### Yacht

```shell
mkdir -p "/root/.local/share/srv/docker/yacht/"
sudo docker run -d \
    --name="yacht" \
    --hostname "yacht" \
    --restart=unless-stopped \
    --privileged \
    -e TZ="${TZ:-${TIMEZONE:-America/New_York}}" \
    -v "/root/.local/share/srv/docker/yacht/data":/data \
    -v "/root/.local/share/srv/docker/yacht/config":/config \
    -p 8000:8000 \
    selfhostedpro/yacht 1>/dev/null
```

### Portainer

```shell
mkdir -p /root/.local/share/srv/docker/portainer/data
docker run -d -p 9000:9000 \
    --name portainer \
    -v /root/.local/share/srv/docker/portainer/data:/data \
    -v /var/run/docker.sock:/var/run/docker.sock \
    portainer/portainer-ce
```

### nginx

```shell
mkdir -p /root/.local/share/srv/docker/nginx-manager/files/{data,config,letsencrypt}
sudo docker run -d \
    --name="nginx-manager" \
    --hostname "$HOSTNAME" \
    --restart=unless-stopped \
    --privileged \
    -e TZ="America/New_York" \
    -e DISABLE_IPV6=true \
    -v "/root/.local/share/srv/docker/nginx-manager/data":/data \
    -v "/root/.local/share/srv/docker/nginx-manager/config":/app/config \
    -v "/root/.local/share/srv/docker/nginx-manager/letsencrypt":/etc/letsencrypt \
    -p 80:80 \
    -p 8888:81 \
    -p 443:443 \
    jc21/nginx-proxy-manager:2
```
