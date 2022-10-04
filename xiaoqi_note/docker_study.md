## 1、安装

#### 1.2环境

系统内核

```
[root@iZbp1d3xjg2fnxprjt05glZ ~]# uname -r
3.10.0-1127.19.1.el7.x86_64

```

系统环境

```
[root@iZbp1d3xjg2fnxprjt05glZ ~]# cat /etc/os-release
NAME="CentOS Linux"
VERSION="7 (Core)"
ID="centos"
ID_LIKE="rhel fedora"
VERSION_ID="7"
PRETTY_NAME="CentOS Linux 7 (Core)"
ANSI_COLOR="0;31"
CPE_NAME="cpe:/o:centos:centos:7"
HOME_URL="https://www.centos.org/"
BUG_REPORT_URL="https://bugs.centos.org/"

CENTOS_MANTISBT_PROJECT="CentOS-7"
CENTOS_MANTISBT_PROJECT_VERSION="7"
REDHAT_SUPPORT_PRODUCT="centos"
REDHAT_SUPPORT_PRODUCT_VERSION="7"

```

安装

> 卸载旧的版本
>
> ```
>  sudo yum remove docker \
>                   docker-client \
>                   docker-client-latest \
>                   docker-common \
>                   docker-latest \
>                   docker-latest-logrotate \
>                   docker-logrotate \
>                   docker-engine
>                   
> # 安装工具
> yum install  -y yum-utils
> # 设置地址
> yum-config-manager \
>     --add-repo \
>     https://download.docker.com/linux/centos/docker-ce.repo // 官网地址
> # 阿里云地址
> yum-config-manager \
> 	--add-repo \
> 	http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
> 	
> # 更新索引
> yum makecache fast
> 
> # 安装docekr
> sudo yum install docker-ce docker-ce-cli containerd.io docker-compose-plugin
> 
> # 启动docker
> systemctl start docker
> # 查看版本
> docker version
> docker info
> 
> #检测是否安装成功
> docker run hello-world
> # 查看镜像
> 	docker images
> # 卸载docker
>  	sudo yum remove docker-ce docker-ce-cli containerd.io docker-compose-plugin
>   	sudo rm -rf /var/lib/docker
>  	sudo rm -rf /var/lib/containerd
> 
> ```

## 阿里云镜像加速

xxxxxx

## 镜像命令

> [root@iZbp1d3xjg2fnxprjt05glZ ~]# docker images
> REPOSITORY    TAG       IMAGE ID       CREATED         SIZE
> hello-world   latest    d1165f221234   19 months ago   13.3kB



> [root@iZbp1d3xjg2fnxprjt05glZ ~]# docker images --help
>
> Usage:  docker images [OPTIONS] [REPOSITORY[:TAG]]
>
> List images
>
> Options:
>   -a, --all             Show all images (default hides intermediate images)
>       --digests         Show digests
>   -f, --filter filter   Filter output based on conditions provided
>       --format string   Pretty-print images using a Go template
>       --no-trunc        Don't truncate output
>   -q, --quiet           Only show image IDs

### docker 搜索镜像

> docker search mysql

```she
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker search mysql
NAME                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                           MySQL is a widely used, open-source relation…   13234     [OK]  

# 追加参数
--filter=stars=6000
```

### docker pull 下载镜像

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker pull --help

Usage:  docker pull [OPTIONS] NAME[:TAG|@DIGEST]

Pull an image or a repository from a registry

Options:
  -a, --all-tags                Download all tagged images in the repository
      --disable-content-trust   Skip image verification (default true)
      --platform string         Set platform if server is multi-platform capable
  -q, --quiet                   Suppress verbose output
[root@iZbp1d3xjg2fnxprjt05glZ ~]# 



[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql
051f419db9dd: Pull complete 
7627573fa82a: Pull complete 
a44b358d7796: Pull complete 
95753aff4b95: Pull complete 
a1fa3bee53f4: Pull complete 
f5227e0d612c: Pull complete 
b4b4368b1983: Pull complete 
f26212810c32: Pull complete 
d803d4215f95: Pull complete 
d5358a7f7d07: Pull complete 
435e8908cd69: Pull complete 
Digest: sha256:b9532b1edea72b6cee12d9f5a78547bd3812ea5db842566e17f8b33291ed2921
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest


#指定版本下载
docker pull mysql:5.7
```

### docker rmi 删除镜像

```shell
docker rmi -f  aa803eda0f25  #指定id 删除
docker rmi -f  id id #空格隔开 删除多个
docker rmi -f  $(docker iamges -aq) #删除全部的镜像
```

## 容器命令

```shell
docker pull centos
```

新建容器并启动

```shell
docker run [参数] image

#参数说明
--name="xxxx" #容器名字
-d            #后台方式运行
-it           #使用交互方式运行
-p            #指定端口
	-p 主机端口：容器端口 常用方式
	-p ip:主机端口：容器端口
	-p 容器端口
-P            #大P 随机指定端口
```

```shell
# 启动并进入容器
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker run -it  centos /bin/bash
# 退出容器
exit
# 查看正在运行的容器
docker ps # 列出正在运行的容器
-a # 列出所有的容器
-n=? #显示最近创建的容器
-q # 只显示容器的编号

[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED         STATUS                      PORTS     NAMES
8278c7d7c49d   centos        "/bin/bash"   3 minutes ago   Exited (0) 54 seconds ago             goofy_burnell
f97901dd0ff1   hello-world   "/hello"      3 hours ago     Exited (0) 3 hours ago                clever_franklin
fc88953b6dda   hello-world   "/hello"      15 months ago   Exited (0) 15 months ago              suspicious_hellman
d916e3a5763a   hello-world   "/hello"      15 months ago   Exited (0) 15 months ago  

```

### 容器退出

```shell
exit
ctrl +P +Q
```

### 删除容器

```shell
docker rm 容器id   #不能删除运行的容器 如果需要删除 加 -f
docker rm -f  $(docker ps -aq)
docker ps -a -qlxargs docker rm -f #删除所有的容器
```

### 启动和停止容器的操作

```shell
docker start 容器id
docker restart 容器id
docker stop 容器id
docker kill 容器id

```

## 其他常用命令

### 后台启动

```shell
#docker run -d  镜像名
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker run -d centos
169b9c49f6978961987fa5bf5b0a6c0fd672aabfee9809fae880f5bfeaf6dfd9
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
# 此处运行失败 docker 使用后台运行 需要前台进程 如果没有前台进程 就会自动停止

```

### 查看日志

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
  -n, --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37Z) or relative (e.g. 42m for 42 minutes)
[root@iZbp1d3xjg2fnxprjt05glZ ~]# 

[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
9bb0520c4f60   centos    "/bin/bash"   20 minutes ago   Up 20 minutes             gracious_agnesi
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker logs -f -t --tail 10 9bb0520c4f60 

```

### 查看容器中的进程

```shell
docker top 容器id
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker top  9bb0520c4f60 
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                21738               21718               0                   Oct04               ?                   00:00:00            /bin/bash
```

### 查看容器的元数据

```shell
docker inspect 容器id
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker inspect  9bb0520c4f60 
[
    {
        "Id": "9bb0520c4f60f70f3dd7180765874641cc967964d06203cbc967ed72d9bf3b6d",
        "Created": "2022-10-04T15:57:43.602062193Z",
        "Path": "/bin/bash",
        "Args": [],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 21738,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-10-04T15:57:43.924787167Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/9bb0520c4f60f70f3dd7180765874641cc967964d06203cbc967ed72d9bf3b6d/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/9bb0520c4f60f70f3dd7180765874641cc967964d06203cbc967ed72d9bf3b6d/hostname",
        "HostsPath": "/var/lib/docker/containers/9bb0520c4f60f70f3dd7180765874641cc967964d06203cbc967ed72d9bf3b6d/hosts",
        "LogPath": "/var/lib/docker/containers/9bb0520c4f60f70f3dd7180765874641cc967964d06203cbc967ed72d9bf3b6d/9bb0520c4f60f70f3dd7180765874641cc967964d06203cbc967ed72d9bf3b6d-json.log",
        "Name": "/gracious_agnesi",
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
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
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
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
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
                "/sys/firmware"
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
                "LowerDir": "/var/lib/docker/overlay2/b14a5797eed9a10854229bd8877fcdcabd90da8fa2cf2fbc4dbe32466d96fa5b-init/diff:/var/lib/docker/overlay2/030af70baf09379ab48e30544e0d56ab1b4a047173c488b463a379bce0a4f081/diff",
                "MergedDir": "/var/lib/docker/overlay2/b14a5797eed9a10854229bd8877fcdcabd90da8fa2cf2fbc4dbe32466d96fa5b/merged",
                "UpperDir": "/var/lib/docker/overlay2/b14a5797eed9a10854229bd8877fcdcabd90da8fa2cf2fbc4dbe32466d96fa5b/diff",
                "WorkDir": "/var/lib/docker/overlay2/b14a5797eed9a10854229bd8877fcdcabd90da8fa2cf2fbc4dbe32466d96fa5b/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "9bb0520c4f60",
            "Domainname": "",
            "User": "",
            "AttachStdin": true,
            "AttachStdout": true,
            "AttachStderr": true,
            "Tty": true,
            "OpenStdin": true,
            "StdinOnce": true,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "51387c04deb0dccbcba3b13ec7cb6a8100981bc95fe3c42b7868e3f6bad7fcad",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/51387c04deb0",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "ced33ed260c7e6e55d57c188e4fb0408f4d95484abfad700b82f1cee82189877",
            "Gateway": "172.18.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.18.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:12:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "9f8b1799a7346caa299f944cf527c761e626dfa02beba1cd616fcd25342fd3c0",
                    "EndpointID": "ced33ed260c7e6e55d57c188e4fb0408f4d95484abfad700b82f1cee82189877",
                    "Gateway": "172.18.0.1",
                    "IPAddress": "172.18.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:12:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]

```

### 进入正在运行的容器

```shell
docker exec -it 容器id /bin/bash  #这种进入方式 会重新开启一个新的端口
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps
CONTAINER ID   IMAGE     COMMAND       CREATED          STATUS          PORTS     NAMES
9bb0520c4f60   centos    "/bin/bash"   38 minutes ago   Up 38 minutes             gracious_agnesi
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker exec -it 9bb0520c4f60 /bin/bash
[root@9bb0520c4f60 /]# 


docker attach 容器id  #这种方式 进去已经运行的容器 不会开启新的端口
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker attach 9bb0520c4f60 
[root@9bb0520c4f60 /]# 

```

### 文件拷贝

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker attach 9bb0520c4f60 
[root@9bb0520c4f60 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@9bb0520c4f60 /]# cd /home
[root@9bb0520c4f60 home]# touch xiaoqi.txt
[root@9bb0520c4f60 home]# ls
xiaoqi.txt
[root@9bb0520c4f60 home]# exit
exit
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps -a
CONTAINER ID   IMAGE     COMMAND        CREATED             STATUS                         PORTS     NAMES
9bb0520c4f60   centos    "/bin/bash"    53 minutes ago      Exited (0) 13 seconds ago                gracious_agnesi
79aada403568   centos    "/bin/bash/"   54 minutes ago      Created                                  recursing_buck
169b9c49f697   centos    "/bin/bash"    About an hour ago   Exited (0) About an hour ago             wizardly_dhawan
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker cp 9bb0520c4f60:/home/xiaoqi.txt /home
[root@iZbp1d3xjg2fnxprjt05glZ ~]# ls
downloads  nginx-1.9.9.tar.gz
[root@iZbp1d3xjg2fnxprjt05glZ ~]# cd ../../
[root@iZbp1d3xjg2fnxprjt05glZ /]# ls
bin  boot  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@iZbp1d3xjg2fnxprjt05glZ /]# cd /home
[root@iZbp1d3xjg2fnxprjt05glZ home]# ls
downloads  mysql  xiaoqi.txt

```

## 部署nginx

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker pull nginx
Using default tag: latest
latest: Pulling from library/nginx
31b3f1ad4ce1: Pull complete 
fd42b079d0f8: Pull complete 
30585fbbebc6: Pull complete 
18f4ffdd25f4: Pull complete 
9dc932c8fba2: Pull complete 
600c24b8ba39: Pull complete 
Digest: sha256:0b970013351304af46f322da1263516b188318682b2ab1091862497591189ff1
Status: Downloaded newer image for nginx:latest
docker.io/library/nginx:latest
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker run -it -d -name nginx001 -p 5011:80 nginx
af56584fb4a0bafce1fb2481d158e455690f3cb22b396369ef0030848dca71e3
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
af56584fb4a0   nginx     "/docker-entrypoint.…"   21 seconds ago   Up 20 seconds   0.0.0.0:5011->80/tcp, :::5011->80/tcp   beautiful_bohr


# 进入容器 查看内容
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker exec -it af56584fb4a0 /bin/bash
root@af56584fb4a0:/# ls
bin   dev                  docker-entrypoint.sh  home  lib64  mnt  proc  run   srv  tmp  var
boot  docker-entrypoint.d  etc                   lib   media  opt  root  sbin  sys  usr
root@af56584fb4a0:/# whereis nginx
nginx: /usr/sbin/nginx /usr/lib/nginx /etc/nginx /usr/share/nginx
root@af56584fb4a0:/# cd /etc/nginx
root@af56584fb4a0:/etc/nginx# 


```

