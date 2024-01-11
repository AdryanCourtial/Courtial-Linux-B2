# 3. sudo c pa bo

Ajouter votre utilisateur au groupe docker : 

```
[baptiste@localhost ~]$ sudo groupadd docker
[sudo] password for baptiste:
groupadd: group 'docker' already exists
[baptiste@localhost ~]$
[baptiste@localhost ~]$
[baptiste@localhost ~]$
[baptiste@localhost ~]$ sudo usermod -aG docker $USER
[baptiste@localhost ~]$ sudo systemctl restart docker
[baptiste@localhost ~]$ docker run hello-world
docker: permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Post "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/create": dial unix /var/run/docker.sock: connect: permission denied.
See 'docker run --help'.
[baptiste@localhost ~]$ docker ps
permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get "http://%2Fvar%2Frun%2Fdocker.sock/v1.24/containers/json": dial unix /var/run/docker.sock: connect: permission denied
[baptiste@localhost ~]$
[baptiste@localhost ~]$ groups
baptiste wheel
[baptiste@localhost ~]$ ^C
[baptiste@localhost ~]$ exit
logout
Connection to 10.1.1.100 closed.
PS C:\Users\happy cash\YnovProjet\B2\Courtial-Linux-B2\TP1> ssh baptiste@10.1.1.100
baptiste@10.1.1.100's password:
Last login: Thu Dec 21 15:51:34 2023 from 10.1.1.1
[baptiste@localhost ~]$ sudo docker ps
[sudo] password for baptiste:
sudo: a password is required
[baptiste@localhost ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[baptiste@localhost ~]$
```

# 4. Un premier conteneur en vif

🌞 Lancer un conteneur NGINX : 

```
[baptiste@localhost ~]$ docker run -d -p 1000:80 nginx
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
af107e978371: Pull complete
336ba1f05c3e: Pull complete
8c37d2ff6efa: Pull complete
51d6357098de: Pull complete
782f1ecce57d: Pull complete
5e99d351b073: Pull complete
7b73345df136: Pull complete
Digest: sha256:bd30b8d47b230de52431cc71c5cce149b8d5d4c87c204902acf2504435d4b4c9
Status: Downloaded newer image for nginx:latest
778e35fc3bc348c6c45c5ee4469cf35093626054cbf28a9c74a1349c4ad7b921
[baptiste@localhost ~]$
```

🌞 Visitons : 

```
[baptiste@localhost ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS                                   NAMES
778e35fc3bc3   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   0.0.0.0:1000->80/tcp, :::1000->80/tcp   festive_varahamihira
[baptiste@localhost ~]$s[baptiste@localhost ~]$ docker logs festive_varahamihira
/docker-entrypoint.sh: /docker-entrypoint.d/ is not empty, will attempt to perform configuration
/docker-entrypoint.sh: Looking for shell scripts in /docker-entrypoint.d/
/docker-entrypoint.sh: Launching /docker-entrypoint.d/10-listen-on-ipv6-by-default.sh
10-listen-on-ipv6-by-default.sh: info: Getting the checksum of /etc/nginx/conf.d/default.conf
10-listen-on-ipv6-by-default.sh: info: Enabled listen on IPv6 in /etc/nginx/conf.d/default.conf
/docker-entrypoint.sh: Sourcing /docker-entrypoint.d/15-local-resolvers.envsh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/20-envsubst-on-templates.sh
/docker-entrypoint.sh: Launching /docker-entrypoint.d/30-tune-worker-processes.sh
/docker-entrypoint.sh: Configuration complete; ready for start up
2023/12/21 15:07:59 [notice] 1#1: using the "epoll" event method
2023/12/21 15:07:59 [notice] 1#1: nginx/1.25.3
2023/12/21 15:07:59 [notice] 1#1: built by gcc 12.2.0 (Debian 12.2.0-14)
2023/12/21 15:07:59 [notice] 1#1: OS: Linux 5.14.0-284.30.1.el9_2.x86_64
2023/12/21 15:07:59 [notice] 1#1: getrlimit(RLIMIT_NOFILE): 1073741816:1073741816
2023/12/21 15:07:59 [notice] 1#1: start worker processes
2023/12/21 15:07:59 [notice] 1#1: start worker process 30
[baptiste@localhost ~]$
[baptiste@localhost ~]$ docker inspect festive_varahamihira
[
    {
        "Id": "778e35fc3bc348c6c45c5ee4469cf35093626054cbf28a9c74a1349c4ad7b921",
        "Created": "2023-12-21T15:07:58.28181938Z",
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
            "Pid": 57505,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2023-12-21T15:07:59.158796344Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:d453dd892d9357f3559b967478ae9cbc417b52de66b53142f6c16c8a275486b9",
        "ResolvConfPath": "/var/lib/docker/containers/778e35fc3bc348c6c45c5ee4469cf35093626054cbf28a9c74a1349c4ad7b921/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/778e35fc3bc348c6c45c5ee4469cf35093626054cbf28a9c74a1349c4ad7b921/hostname",
        "HostsPath": "/var/lib/docker/containers/778e35fc3bc348c6c45c5ee4469cf35093626054cbf28a9c74a1349c4ad7b921/hosts",
        "LogPath": "/var/lib/docker/containers/778e35fc3bc348c6c45c5ee4469cf35093626054cbf28a9c74a1349c4ad7b921/778e35fc3bc348c6c45c5ee4469cf35093626054cbf28a9c74a1349c4ad7b921-json.log",
        "Name": "/festive_varahamihira",
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

                    [...] 

                    "NetworkID": "ede463b82385aef1c94b938c785d4fa2d1e2781dc46ee63c0ba397b3b32c2306",
                    "EndpointID": "9cbb99ba5ae98a7b34c350860520b653bdd59dfcc35c3e78c07e1b35c0c00a6d",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
]
[baptiste@localhost ~]$
[baptiste@localhost ~]$ firewall-cmd --zone=public --add-port=1000/tcp --permanent
ERROR:dbus.proxies:Introspect error on :1.22:/org/fedoraproject/FirewallD1/config: dbus.exceptions.DBusException: org.fedoraproject.FirewallD1.NotAuthorizedException: Not Authorized(uid): org.fedoraproject.FirewallD1.info
Authorization failed.
    Make sure polkit agent is running or run the application as superuser.
[baptiste@localhost ~]$ sudo firewall-cmd --zone=public --add-port=1000/tcp --permanent
success
[baptiste@localhost ~]$ sudo firewall-cmd list
usage: 'firewall-cmd --help' for usage information or see firewall-cmd(1) man page
firewall-cmd: error: unrecognized arguments: list
[baptiste@localhost ~]$ sudo list firewall-cmd
sudo: list: command not found
[baptiste@localhost ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports:
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
[baptiste@localhost ~]$ sudo systemctl reload firewalld
[baptiste@localhost ~]$ sudo firewall-cmd --list-all
public (active)
  target: default
  icmp-block-inversion: no
  interfaces: enp0s3 enp0s8
  sources:
  services: cockpit dhcpv6-client ssh
  ports: 1000/tcp
  protocols:
  forward: yes
  masquerade: no
  forward-ports:
  source-ports:
  icmp-blocks:
  rich rules:
[baptiste@localhost ~]$
[baptiste@localhost ~]$ curl 10.1.1.100:1000
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
[baptiste@localhost ~]$
```

🌞 Visitons

```
[baptiste@localhost ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED              STATUS              PORTS
                       NAMES
5dcd9460d297   nginx     "/docker-entrypoint.…"   About a minute ago   Up About a minute   80/tcp, 0.0.0.0:1000->8080/tcp, :::1000->8080/tcp   musing_hoover
[baptiste@localhost ~]$ curl 10.1.1.100:1000
<h1>MEOOOW</h1>[baptiste@localhost ~]$
```

# 5. Un deuxième conteneur en vif

🌞 Lance un conteneur Python, avec un shell

```
[baptiste@localhost ~]$ docker run -it python bash
Unable to find image 'python:latest' locally
latest: Pulling from library/python
bc0734b949dc: Pull complete
b5de22c0f5cd: Pull complete
917ee5330e73: Pull complete
b43bd898d5fb: Pull complete
7fad4bffde24: Pull complete
d685eb68699f: Pull complete
107007f161d0: Pull complete
02b85463d724: Pull complete
Digest: sha256:3733015cdd1bd7d9a0b9fe21a925b608de82131aa4f3d397e465a1fcb545d36f
Status: Downloaded newer image for python:latest
root@7db6267fc9b5:/#
```

🌞 Installe des libs Python

```
root@7db6267fc9b5:/# pip install aiohttp
Collecting aiohttp
  Obtaining dependency information for aiohttp from https://files.pythonhosted.org/packages/75/5f/90a2869ad3d1fb9bd984bfc1b02d8b19e381467b238bd3668a09faa69df5/aiohttp-3.9.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata

    [...]

[notice] A new release of pip is available: 23.2.1 -> 23.3.2
[notice] To update, run: pip install --upgrade pip
root@7db6267fc9b5:/# pip install aioconsole
Collecting aioconsole
  Obtaining dependency information for aioconsole from https://files.pythonhosted.org/packages/f7/39/b392dc1a8bb58342deacc1ed2b00edf88fd357e6fdf76cc6c8046825f84f/aioconsole-0.7.0-py3-none-any.whl.metadata
  Downloading aioconsole-0.7.0-py3-none-any.whl.metadata (5.3 kB)
Downloading aioconsole-0.7.0-py3-none-any.whl (30 kB)
Installing collected packages: aioconsole
Successfully installed aioconsole-0.7.0
WARNING: Running pip as the 'root' user can result in broken permissions and conflicting behaviour with the system package manager. It is recommended to use a virtual environment instead: https://pip.pypa.io/warnings/venv

[notice] A new release of pip is available: 23.2.1 -> 23.3.2
[notice] To update, run: pip install --upgrade pip
root@7db6267fc9b5:/# python
Python 3.12.1 (main, Dec 19 2023, 20:14:15) [GCC 12.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import aiohttp
```



## II. Images

# 1. Images publiques

```
[baptiste@localhost ~]$ docker pull python:3.11
3.11: Pulling from library/python
bc0734b949dc: Already exists
[...]
fefdcd9854bf: Pull complete
Digest: sha256:4e5e9b05dda9cf699084f20bb1d3463234446387fa0f7a45d90689c48e204c83
Status: Downloaded newer image for python:3.11
docker.io/library/python:3.11
[baptiste@localhost ~]$ docker mysql5.7
docker: 'mysql5.7' is not a docker command.
See 'docker --help'
[baptiste@localhost ~]$ docker pull mysql5.7
Using default tag: latest
Error response from daemon: pull access denied for mysql5.7, repository does not exist or may require 'docker login': denied: requested access to the resource is denied
[baptiste@localhost ~]$ ^C
[baptiste@localhost ~]$ docker pull mysql 5.7.32
"docker pull" requires exactly 1 argument.
See 'docker pull --help'.

Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Download an image from a registry
[baptiste@localhost ~]$ docker pull mysql5.7.32
Using default tag: latest
^C
[baptiste@localhost ~]$ docker pull mysql:5.7.32
5.7.32: Pulling from library/mysql
a076a628af6f: Pull complete
[...]
8c3e9d7c9815: Pull complete
fadfb9734ed2: Pull complete
Digest: sha256:e08834258fcc0efd01df358222333919df53d4a0d9b2a54da05b204b822e3b7b
Status: Downloaded newer image for mysql:5.7.32
docker.io/library/mysql:5.7.32
[baptiste@localhost ~]$ docker pull wordpress
Using default tag: latest
latest: Pulling from library/wordpress
af107e978371: Already exists
6480d4ad61d2: Pull complete
95f5176ece8b: Pull complete
[...]
a1ff013e1d94: Pull complete
31076364071c: Pull complete
87728bbad961: Pull complete
Digest: sha256:ffabdfe91eefc08f9675fe0e0073b2ebffa8a62264358820bcf7319b6dc09611
Status: Downloaded newer image for wordpress:latest
docker.io/library/wordpress:latest
[baptiste@localhost ~]$ docker pull linuxserver/wikijs
Using default tag: latest
latest: Pulling from linuxserver/wikijs
8b16ab80b9bd: Pull complete
[...]
20b23561c7ea: Pull complete
Digest: sha256:131d247ab257cc3de56232b75917d6f4e24e07c461c9481b0e7072ae8eba3071
Status: Downloaded newer image for linuxserver/wikijs:latest
docker.io/linuxserver/wikijs:latest
[baptiste@localhost ~]$
[baptiste@localhost ~]$ docker images
REPOSITORY           TAG       IMAGE ID       CREATED        SIZE
linuxserver/wikijs   latest    869729f6d3c5   7 days ago     441MB
python               latest    fc7a60e86bae   2 weeks ago    1.02GB
wordpress            latest    fd2f5a0c6fba   2 weeks ago    739MB
python               3.11      22140cbb3b0c   2 weeks ago    1.01GB
nginx                latest    d453dd892d93   8 weeks ago    187MB
hello-world          latest    d2c94e258dcb   7 months ago   13.3kB
mysql                5.7.32    cc8775c0fe94   2 years ago    449MB
[baptiste@localhost ~]$
```

```
[baptiste@localhost ~]$ docker run -it python:3.11 bash
root@56eabef8682f:/#
root@56eabef8682f:/# python --version
Python 3.11.7
root@56eabef8682f:/#
```