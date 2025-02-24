# TP1 : Premiers pas Docker

Dans ce TP on va appréhender les bases de Docker.

Etant fondamentalement une techno Linux, **vous réaliserez le TP sur une VM Linux** (ou sur votre poste si vous êtes sur Linux).

> *Oui oui les dévs, on utilisera Docker avec vos Windows/MacOS plus tard. Là on va se concentrer sur l'essence du truc.*

## Sommaire

- [TP1 : Premiers pas Docker](#tp1--premiers-pas-docker)
  - [Sommaire](#sommaire)
- [0. Setup](#0-setup)
- [I. Init](#i-init)
- [II. Images](#ii-images)
- [III. Docker compose](#iii-docker-compose)

# 0. Setup

➜ **Munissez-vous du [mémo Docker](../../cours/memo/docker.md)**

➜ **Une VM Rocky Linux sitoplé, une seul suffit**

- met lui une carte host-only pour pouvoir SSH dessus
- et une carte NAT pour un accès internet

➜ **Checklist habituelle :**

- [x] IP locale, statique ou dynamique
- [x] hostname défini
- [x] SSH fonctionnel
- [x] accès Internet
- [x] résolution de nom
- [x] SELinux en mode *"permissive"* vérifiez avec `sestatus`, voir [mémo install VM tout en bas](../../cours/memo/install_vm.md)

# I. Init

- [I. Init](#i-init)
  - [1. Installation de Docker](#1-installation-de-docker)
  - [2. Vérifier que Docker est bien là](#2-vérifier-que-docker-est-bien-là)
  - [3. sudo c pa bo](#3-sudo-c-pa-bo)
  - [4. Un premier conteneur en vif](#4-un-premier-conteneur-en-vif)
  - [5. Un deuxième conteneur en vif](#5-un-deuxième-conteneur-en-vif)

## 1. Installation de Docker

```bash
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```
```bash
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo systemctl enable --now docker
```

## 2. Vérifier que Docker est bien là

```bash
[et0@tp1 ~]$ systemctl status docker
● docker.service - Docker Application Container Engine
     Loaded: loaded (/usr/lib/systemd/system/docker.service; enabled; preset: disabled)
     Active: active (running) since Thu 2024-12-12 09:39:07 CET; 13min ago
TriggeredBy: ● docker.socket
       Docs: https://docs.docker.com
   Main PID: 12781 (dockerd)
      Tasks: 11
     Memory: 41.8M
        CPU: 391ms
     CGroup: /system.slice/docker.service
             └─12781 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Dec 12 09:39:06 tp1 dockerd[12781]: time="2024-12-12T09:39:06.715450395+01:00" level=info >
Dec 12 09:39:07 tp1 dockerd[12781]: time="2024-12-12T09:39:07.230622262+01:00" level=info >
Dec 12 09:39:07 tp1 dockerd[12781]: time="2024-12-12T09:39:07.356962381+01:00" level=info >
Dec 12 09:39:07 tp1 dockerd[12781]: time="2024-12-12T09:39:07.365663849+01:00" level=warni>
Dec 12 09:39:07 tp1 dockerd[12781]: time="2024-12-12T09:39:07.365801388+01:00" level=warni>
Dec 12 09:39:07 tp1 dockerd[12781]: time="2024-12-12T09:39:07.365830368+01:00" level=info >
Dec 12 09:39:07 tp1 dockerd[12781]: time="2024-12-12T09:39:07.365954184+01:00" level=info >
Dec 12 09:39:07 tp1 dockerd[12781]: time="2024-12-12T09:39:07.387047426+01:00" level=info >
Dec 12 09:39:07 tp1 systemd[1]: Started Docker Application Container Engine.
Dec 12 09:39:17 tp1 dockerd[12781]: time="2024-12-12T09:39:17.164232736+01:00" level=info >
```
```bash
[et0@tp1 ~]$ docker info
Client: Docker Engine - Community
 Version:    27.4.0
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.19.2
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v2.31.0
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 1
 Server Version: 27.4.0
 Storage Driver: overlay2
  Backing Filesystem: xfs
  Supports d_type: true
  Using metacopy: false
  Native Overlay Diff: true
  userxattr: false
 Logging Driver: json-file
 Cgroup Driver: systemd
 Cgroup Version: 2
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local splunk syslog
 Swarm: inactive
 Runtimes: io.containerd.runc.v2 runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 88bf19b2105c8b17560993bee28a01ddc2f97182
 runc version: v1.2.2-0-g7cb3632
 init version: de40ad0
 Security Options:
  seccomp
   Profile: builtin
  cgroupns
 Kernel Version: 5.14.0-427.13.1.el9_4.aarch64
 Operating System: Rocky Linux 9.4 (Blue Onyx)
 OSType: linux
 Architecture: aarch64
 CPUs: 1
 Total Memory: 635.6MiB
 Name: tp1
 ID: c9b09697-5890-4f1f-8af6-75acbf5eb047
 Docker Root Dir: /var/lib/docker
 Debug Mode: false
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false

WARNING: bridge-nf-call-iptables is disabled
WARNING: bridge-nf-call-ip6tables is disabled
```
```bash
[et0@tp1 ~]$ sudo docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

## 3. sudo c pa bo

🌞 **Ajouter votre utilisateur au groupe `docker`**

```bash
sudo usermod -aG docker ${USER}
su - ${USER}
```

Genre si tu trouves que taper `docker` c'est long, et tu préférerais taper `dk` tu peux faire : `alias dk='docker'`. Si tu écris cette commande dans ton fichier `~/.bashrc` alors ce sera effectif dans n'importe quel `bash` que tu ouvriras plutar.

## 4. Un premier conteneur en vif

🌞 **Lancer un conteneur NGINX**

- avec la commande suivante :

```bash
[et0@tp1 ~]$ docker run -d -p 9999:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
bb3f2b52e6af: Pull complete 
e4bc5c1a6721: Pull complete 
e93f7200eab8: Pull complete 
1bd52ec2c0cb: Pull complete 
411a98463f95: Pull complete 
ad5932596f78: Pull complete 
df25b2e5edb3: Pull complete 
Digest: sha256:fb197595ebe76b9c0c14ab68159fd3c08bd067ec62300583543f0ebda353b5be
Status: Downloaded newer image for nginx:latest
3aa39a21695f5f3611f4ea823e81f32a9fb5e3fc854f16cb21fee6192e55fa25
```

🌞 **Visitons**

```bash
[et0@tp1 ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS                                     NAMES
3aa39a21695f   nginx     "/docker-entrypoint.…"   5 minutes ago   Up 5 minutes   0.0.0.0:9999->80/tcp, [::]:9999->80/tcp   epic_gagarin
```
```bash
[et0@tp1 ~]$ docker logs 3aa39a21695f
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2024/12/12 08:57:23 [notice] 1#1: using the "epoll" event method
2024/12/12 08:57:23 [notice] 1#1: nginx/1.27.3
2024/12/12 08:57:23 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14) 
2024/12/12 08:57:23 [notice] 1#1: OS: Linux 5.14.0-427.13.1.el9_4.aarch64
2024/12/12 08:57:23 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1073741816:1073741816
2024/12/12 08:57:23 [notice] 1#1: start worker processes
2024/12/12 08:57:23 [notice] 1#1: start worker process 28
```
```bash
[et0@tp1 ~]$ docker inspect 3aa39a21695f
[
    {
        "Id": "3aa39a21695f5f3611f4ea823e81f32a9fb5e3fc854f16cb21fee6192e55fa25",
        "Created": "2024-12-12T08:57:23.07462339Z",
        "Path": "/docker-entrypoint.sh",
        "Args": [
            "nginx",
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
            "Pid": 13287,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2024-12-12T08:57:23.209506542Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:bdf62fd3a32f1209270ede068b6e08450dfe125c79b1a8ba8f5685090023bf7f",
        "ResolvConfPath": "/var/lib/docker/containers/3aa39a21695f5f3611f4ea823e81f32a9fb5e3fc854f16cb21fee6192e55fa25/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/3aa39a21695f5f3611f4ea823e81f32a9fb5e3fc854f16cb21fee6192e55fa25/hostname",
        "HostsPath": "/var/lib/docker/containers/3aa39a21695f5f3611f4ea823e81f32a9fb5e3fc854f16cb21fee6192e55fa25/hosts",
        "LogPath": "/var/lib/docker/containers/3aa39a21695f5f3611f4ea823e81f32a9fb5e3fc854f16cb21fee6192e55fa25/3aa39a21695f5f3611f4ea823e81f32a9fb5e3fc854f16cb21fee6192e55fa25-json.log",
        "Name": "/epic_gagarin",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "bridge",
            "PortBindings": {
                "80/tcp": [
                    {
                        "HostIp": "",
                        "HostPort": "9999"
                    }
                ]
            },
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "ConsoleSize": [
                49,
                91
            ],
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "private",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
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
            "Runtime": "runc",
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": [],
            "BlkioDeviceWriteBps": [],
            "BlkioDeviceReadIOps": [],
            "BlkioDeviceWriteIOps": [],
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": null,
            "PidsLimit": null,
            "Ulimits": [],
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware",
                "/sys/devices/virtual/powercap"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/f9ccaa74c31a230e594c8915fb8a9b5b7716fd37477157fce69c9a9aa56c53a3-init/diff:/var/lib/docker/overlay2/efcf072b903ba2b79ba926a2eb843e3e85d86ad5986e69afdf059762c04918e4/diff:/var/lib/docker/overlay2/5bac7fa07a26b99a2438cb8ec9e0329f4e3e3773d0ce747803d13015227e5e18/diff:/var/lib/docker/overlay2/e47d1d0f5ebf6cb22af2f9e53dfc7fc6ce5bee6a9ffe464a031a3c18c6ef5924/diff:/var/lib/docker/overlay2/8b857e54100d91958b5f8a9b00dbe828652901e1c370005e5b56f05921fc7ca5/diff:/var/lib/docker/overlay2/6d1b355374c35e26710ee05e4d4a433d0d9fa6472028adcd144f1e2cb569cf72/diff:/var/lib/docker/overlay2/5c13efa63dd2c117643ddf6d858008f243f43756196134f8b9cca650171b80e1/diff:/var/lib/docker/overlay2/6a3e372f5a6bb64ae91d4a18ddfcea166f1b04c2fbdec2a95ba03a221f6070a3/diff",
                "MergedDir": "/var/lib/docker/overlay2/f9ccaa74c31a230e594c8915fb8a9b5b7716fd37477157fce69c9a9aa56c53a3/merged",
                "UpperDir": "/var/lib/docker/overlay2/f9ccaa74c31a230e594c8915fb8a9b5b7716fd37477157fce69c9a9aa56c53a3/diff",
                "WorkDir": "/var/lib/docker/overlay2/f9ccaa74c31a230e594c8915fb8a9b5b7716fd37477157fce69c9a9aa56c53a3/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "3aa39a21695f",
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
                "NGINX_VERSION=1.27.3",
                "NJS_VERSION=0.8.7",
                "NJS_RELEASE=1~bookworm",
                "PKG_RELEASE=1~bookworm",
                "DYNPKG_RELEASE=1~bookworm"
            ],
            "Cmd": [
                "nginx",
                "-g",
                "daemon off;"
            ],
            "Image": "nginx",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": [
                "/docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {
                "maintainer": "NGINX Docker Maintainers <docker-maint@nginx.com>"
            },
            "StopSignal": "SIGQUIT"
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "38bcb37c5e88f31eb0c73c49213d2228ce435ce2d0f4b80d0fc4cbe63c928ece",
            "SandboxKey": "/var/run/docker/netns/38bcb37c5e88",
            "Ports": {
                "80/tcp": [
                    {
                        "HostIp": "0.0.0.0",
                        "HostPort": "9999"
                    },
                    {
                        "HostIp": "::",
                        "HostPort": "9999"
                    }
                ]
            },
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "c1a41026b945a09a3d5608f9c68f01bd56c28dea3571ba11c184aa202ce2b3de",
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
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null,
                    "NetworkID": "5b36f01baa1725070a648809c111e6d8c3fef09089668aad8b9e18881ec53ebc",
                    "EndpointID": "c1a41026b945a09a3d5608f9c68f01bd56c28dea3571ba11c184aa202ce2b3de",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "DNSNames": null
                }
            }
        }
    }
]

```
```bash
[et0@tp1 ~]$ sudo ss -lnpt | grep docker
LISTEN 0      4096         0.0.0.0:9999      0.0.0.0:*    users:(("docker-proxy",pid=13237,fd=4))
LISTEN 0      4096            [::]:9999         [::]:*    users:(("docker-proxy",pid=13242,fd=4))
```
```bash
[et0@tp1 ~]$ sudo firewall-cmd --add-port=9999/tcp --permanent
success
[et0@tp1 ~]$ sudo firewall-cmd --reload
success
```
```bash
stan@Stanislass-MacBook-Pro-2 ~ % curl 10.1.1.11:9999
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

🌞 **On va ajouter un site Web au conteneur NGINX**

```bash
[et0@tp1 nginx]$ mkdir -p /home/patron/nginx
[et0@tp1 nginx]$ cd /home/patron/nginx
[et0@tp1 nginx]$ echo '<h1>MEOOOW</h1>' > index.html
[et0@tp1 nginx]$ cat <<EOL > site_nul.conf
server {
    listen        8080;

    location / {
        root /var/www/html;
    }
}
EOL
[et0@tp1 nginx]$ docker run -d -p 9999:8080 -v /home/patron/nginx/index.html:/var/www/html/index.html -v /home/patron/nginx/site_nul.conf:/etc/nginx/conf.d/site_nul.conf nginx
```

🌞 **Visitons**

```bash
stan@Stanislass-MacBook-Pro-2 ~ % curl 10.1.1.11:9999
<h1>MEOOOW</h1>
```

## 5. Un deuxième conteneur en vif

🌞 **Lance un conteneur Python, avec un shell**

```bash
[et0@tp1 nginx]$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
82312fccb35f: Pull complete 
4ac722d9cf93: Pull complete 
261351ed796d: Pull complete 
a9d319298afc: Pull complete 
f5a1b41a75a4: Pull complete 
f1231df76941: Pull complete 
1f1e5190fee2: Pull complete 
Digest: sha256:9255d1993f6d28b8a1cd611b108adbdfa38cb7ccc46ddde8ea7d734b6c845e32
Status: Downloaded newer image for python:latest
root@a3e245f6fe7f:/# 
```

🌞 **Installe des libs Python**

```bash
root@a3e245f6fe7f:/# pip install aiohttp
Collecting aiohttp
  Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (7.7 kB)
Collecting aiohappyeyeballs>=2.3.0 (from aiohttp)
  Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl.metadata (6.1 kB)
Collecting aiosignal>=1.1.2 (from aiohttp)
  Downloading aiosignal-1.3.1-py3-none-any.whl.metadata (4.0 kB)
Collecting attrs>=17.3.0 (from aiohttp)
  Downloading attrs-24.2.0-py3-none-any.whl.metadata (11 kB)
Collecting frozenlist>=1.1.1 (from aiohttp)
  Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (13 kB)
Collecting multidict<7.0,>=4.5 (from aiohttp)
  Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (5.0 kB)
Collecting propcache>=0.2.0 (from aiohttp)
  Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (9.2 kB)
Collecting yarl<2.0,>=1.17.0 (from aiohttp)
  Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata (69 kB)
Collecting idna>=2.0 (from yarl<2.0,>=1.17.0->aiohttp)
  Downloading idna-3.10-py3-none-any.whl.metadata (10 kB)
Downloading aiohttp-3.11.10-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (1.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 1.7/1.7 MB 8.5 MB/s eta 0:00:00
Downloading aiohappyeyeballs-2.4.4-py3-none-any.whl (14 kB)
Downloading aiosignal-1.3.1-py3-none-any.whl (7.6 kB)
Downloading attrs-24.2.0-py3-none-any.whl (63 kB)
Downloading frozenlist-1.5.0-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (265 kB)
Downloading multidict-6.1.0-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (130 kB)
Downloading propcache-0.2.1-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (224 kB)
Downloading yarl-1.18.3-cp313-cp313-manylinux_2_17_aarch64.manylinux2014_aarch64.whl (333 kB)
Downloading idna-3.10-py3-none-any.whl (70 kB)
Installing collected packages: propcache, multidict, idna, frozenlist, attrs, aiohappyeyeballs, yarl, aiosignal, aiohttp
Successfully installed aiohappyeyeballs-2.4.4 aiohttp-3.11.10 aiosignal-1.3.1 attrs-24.2.0 frozenlist-1.5.0 idna-3.10 multidict-6.1.0 propcache-0.2.1 yarl-1.18.3
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
```
```bash
root@a3e245f6fe7f:/# pip install aioconsole
Collecting aioconsole
  Downloading aioconsole-0.8.1-py3-none-any.whl.metadata (46 kB)
Downloading aioconsole-0.8.1-py3-none-any.whl (43 kB)
Installing collected packages: aioconsole
Successfully installed aioconsole-0.8.1
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager, possibly rendering your system unusable.It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv. Use the --root-user-action option if you know what you are doing and want to suppress this warning.
```
```bash
root@a3e245f6fe7f:/# python
Python 3.13.1 (main, Dec  4 2024, 20:55:07) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import aiohttp
>>> 
```
# II. Images

# II. Images

- [II. Images](#ii-images)
  - [1. Images publiques](#1-images-publiques)
  - [2. Construire une image](#2-construire-une-image)

## 1. Images publiques

🌞 **Récupérez des images**

```bash
[et0@tp1 nginx]$ docker pull python:3.13
3.13: Pulling from library/python
Digest: sha256:9255d1993f6d28b8a1cd611b108adbdfa38cb7ccc46ddde8ea7d734b6c845e32
Status: Downloaded newer image for python:3.13
docker.io/library/python:3.13
```
```bash
[et0@tp1 nginx]$ docker pull mysql:5.7
5.7: Pulling from library/mysql
no matching manifest for linux/arm64/v8 in the manifest list entries
```
```bash
[et0@tp1 nginx]$ docker pull wordpress
Using default tag: latest
latest: Pulling from library/wordpress
bb3f2b52e6af: Already exists 
593b3b5e04c8: Pull complete 
dfc980920c05: Pull complete 
7130ec097640: Pull complete 
e0487dfea6ac: Pull complete 
9970f3d36c66: Pull complete 
78fe821f4633: Pull complete 
c7f5d857df6c: Pull complete 
f66f7bb88ea9: Pull complete 
52087d0e8882: Pull complete 
b790ab7de78e: Pull complete 
36982426b87e: Pull complete 
19b1d11241f9: Pull complete 
4f4fb700ef54: Pull complete 
778a803c7c10: Pull complete 
e33aed17206b: Pull complete 
68a0ba65b9a8: Pull complete 
e6f0e8572b0d: Pull complete 
84ae0100b469: Pull complete 
403961fe0b19: Pull complete 
c735898fe033: Pull complete 
f262673756c8: Pull complete 
Digest: sha256:2f3572d5cd722489fe47d59ed45d947dc9507f5a019eb0567ddf24aedf9257ed
Status: Downloaded newer image for wordpress:latest
docker.io/library/wordpress:latest
```
```bash
[et0@tp1 nginx]$ docker pull linuxserver/wikijs
Using default tag: latest
latest: Pulling from linuxserver/wikijs
933485f4c276: Pull complete 
ec7680b185bf: Pull complete 
d2f6df0c6215: Pull complete 
3c2257692d35: Pull complete 
e239f354149a: Pull complete 
f28a39fb8817: Pull complete 
44fb761f238f: Pull complete 
ca693d1d8c0a: Pull complete 
Digest: sha256:45ecf23a3bf849a0bf0c9e2d832aede159e98efd29fef70bbc7ff4dd87522eba
Status: Downloaded newer image for linuxserver/wikijs:latest
docker.io/linuxserver/wikijs:latest
```
```bash
[et0@tp1 nginx]$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED         SIZE
linuxserver/wikijs   latest    c8e0433f5684   5 days ago      480MB
python               3.13      0ef11a309de4   8 days ago      1.02GB
nginx                latest    bdf62fd3a32f   2 weeks ago     197MB
wordpress            latest    9671ee3ca0d9   2 weeks ago     696MB
```

🌞 **Lancez un conteneur à partir de l'image Python**

```bash
[et0@tp1 ~]$ docker run -it python:3.13 bash
root@fe40eb0fd4fa:/# python --version
Python 3.13.1
```

## 2. Construire une image

🌞 **Ecrire un Dockerfile pour une image qui héberge une application Python**

```bash
[et0@tp1 python_app_build]$ sudo cat app.py 
import emoji

print(emoji.emojize("Cet exemple d'application est vraiment naze :thumbs_down:"))

[et0@tp1 python_app_build]$ sudo cat Dockerfile 
FROM debian

RUN apt update

RUN apt install -y python3 python3-pip

RUN pip install emoji

COPY . .

ENTRYPOINT ["python3", "app.py"]
```

🌞 **Build l'image**
```bash
[et0@tp1 python_app_build]$ docker build . --no-cache -t python_app:version_de_ouf
[+] Building 31.6s (10/10) FINISHED                                         docker:default
 => [internal] load build definition from Dockerfile                                  0.0s
 => => transferring dockerfile: 241B                                                  0.0s
 => [internal] load metadata for docker.io/library/debian:latest                      0.0s
 => [internal] load .dockerignore                                                     0.0s
 => => transferring context: 2B                                                       0.0s
 => CACHED [1/5] FROM docker.io/library/debian:latest                                 0.0s
 => [internal] load build context                                                     0.0s
 => => transferring context: 325B                                                     0.0s
 => [2/5] RUN apt update                                                              2.2s
 => [3/5] RUN apt install -y python3 python3-pip                                     25.3s 
 => [4/5] RUN apt install python3-emoji                                               1.3s 
 => [5/5] COPY . .                                                                    0.0s 
 => exporting to image                                                                2.7s 
 => => exporting layers                                                               2.7s 
 => => writing image sha256:3d19cffe08e3d33c4d64625e96b1add40f0af195dade01be21227ea6  0.0s 
 => => naming to docker.io/library/python_app:version_de_ouf                          0.0s 
```

🌞 **Lancer l'image**
```bash
[et0@tp1 ~]$ docker run python_app:version_de_ouf
Cet exemple d'application est vraiment naze 👎
```

# III. Docker compose

Pour la fin de ce TP on va manipuler un peu `docker compose`.

🌞 **Créez un fichier `docker-compose.yml`**
```bash
[et0@tp1 ~]$ cat compose_test/docker-compose.yml 
version: "3"

services:
  conteneur_nul:
    image: debian
    entrypoint: sleep 9999
  conteneur_flopesque:
    image: debian
    entrypoint: sleep 9999
```

🌞 **Lancez les deux conteneurs** avec `docker compose`

```bash
[et0@tp1 ~]$ cd compose_test/ && docker compose up -d
WARN[0000] /home/patron/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
[+] Running 3/3
 ✔ Network compose_test_default                  Cr...                                0.4s 
 ✔ Container compose_test-conteneur_nul-1        Started                              0.4s 
 ✔ Container compose_test-conteneur_flopesque-1  Started                              0.4s 
```

🌞 **Vérifier que les deux conteneurs tournent**

```bash
[et0@tp1 compose_test]$ docker compose ps
WARN[0000] /home/patron/compose_test/docker-compose.yml: the attribute `version` is obsolete, it will be ignored, please remove it to avoid potential confusion 
NAME                                 IMAGE     COMMAND        SERVICE               CREATED              STATUS              PORTS
compose_test-conteneur_flopesque-1   debian    "sleep 9999"   conteneur_flopesque   About a minute ago   Up About a minute   
compose_test-conteneur_nul-1         debian    "sleep 9999"   conteneur_nul         About a minute ago   Up About a minute  
```

🌞 **Pop un shell dans le conteneur `conteneur_nul`**

```bash
[et0@tp1 compose_test]$ docker exec -it compose_test-conteneur_nul-1 bash
root@1d02bda595ea:/# apt-get update
Hit:1 http://deb.debian.org/debian bookworm InRelease
Get:2 http://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
Get:3 http://deb.debian.org/debian-security bookworm-security InRelease [48.0 kB]
Get:4 http://deb.debian.org/debian bookworm-updates/main arm64 Packages [8844 B]
Get:5 http://deb.debian.org/debian-security bookworm-security/main arm64 Packages [213 kB]
Fetched 222 kB in 0s (635 kB/s)
Reading package lists... Done
root@1d02bda595ea:/# apt-get install -y iputils-ping
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
The following additional packages will be installed:
  libcap2-bin libpam-cap
The following NEW packages will be installed:
  iputils-ping libcap2-bin libpam-cap
0 upgraded, 3 newly installed, 0 to remove and 0 not upgraded.
Need to get 94.6 kB of archives.
After this operation, 598 kB of additional disk space will be used.
Get:1 http://deb.debian.org/debian bookworm/main arm64 libcap2-bin arm64 1:2.66-4 [33.9 kB]
Get:2 http://deb.debian.org/debian bookworm/main arm64 iputils-ping arm64 3:20221126-1+deb12u1 [46.1 kB]
Get:3 http://deb.debian.org/debian bookworm/main arm64 libpam-cap arm64 1:2.66-4 [14.5 kB]
Fetched 94.6 kB in 0s (469 kB/s) 
debconf: delaying package configuration, since apt-utils is not installed
Selecting previously unselected package libcap2-bin.
(Reading database ... 6083 files and directories currently installed.)
Preparing to unpack .../libcap2-bin_1%3a2.66-4_arm64.deb ...
Unpacking libcap2-bin (1:2.66-4) ...
Selecting previously unselected package iputils-ping.
Preparing to unpack .../iputils-ping_3%3a20221126-1+deb12u1_arm64.deb ...
Unpacking iputils-ping (3:20221126-1+deb12u1) ...
Selecting previously unselected package libpam-cap:arm64.
Preparing to unpack .../libpam-cap_1%3a2.66-4_arm64.deb ...
Unpacking libpam-cap:arm64 (1:2.66-4) ...
Setting up libcap2-bin (1:2.66-4) ...
Setting up libpam-cap:arm64 (1:2.66-4) ...
debconf: unable to initialize frontend: Dialog
debconf: (No usable dialog-like program is installed, so the dialog based frontend cannot be used. at /usr/share/perl5/Debconf/FrontEnd/Dialog.pm line 78.)
debconf: falling back to frontend: Readline
debconf: unable to initialize frontend: Readline
debconf: (Can't locate Term/ReadLine.pm in @INC (you may need to install the Term::ReadLine module) (@INC contains: /etc/perl /usr/local/lib/aarch64-linux-gnu/perl/5.36.0 /usr/local/share/perl/5.36.0 /usr/lib/aarch64-linux-gnu/perl5/5.36 /usr/share/perl5 /usr/lib/aarch64-linux-gnu/perl-base /usr/lib/aarch64-linux-gnu/perl/5.36 /usr/share/perl/5.36 /usr/local/lib/site_perl) at /usr/share/perl5/Debconf/FrontEnd/Readline.pm line 7.)
debconf: falling back to frontend: Teletype
Setting up iputils-ping (3:20221126-1+deb12u1) ...
root@1d02bda595ea:/# ping conteneur_flopesque
PING conteneur_flopesque (172.18.0.2) 56(84) bytes of data.
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=1 ttl=64 time=0.266 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=2 ttl=64 time=0.299 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=3 ttl=64 time=0.396 ms
64 bytes from compose_test-conteneur_flopesque-1.compose_test_default (172.18.0.2): icmp_seq=4 ttl=64 time=0.334 ms
^C
--- conteneur_flopesque ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3011ms
rtt min/avg/max/mdev = 0.266/0.323/0.396/0.048 ms
```