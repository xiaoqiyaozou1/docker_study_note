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

![image-20221005162241670](C:\Users\qishou\AppData\Roaming\Typora\typora-user-images\image-20221005162241670.png)

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

## 容器数据卷

> 命令挂载
>
> ```she
> docker run -it -v 主机目录:容器目录 
> 
> [root@iZbp1d3xjg2fnxprjt05glZ home]# docker run -it -v /home/xiaoqi_dir:/home centos /bin/bash
> [root@7d066d1bf6ed /]# [root@iZbp1d3xjg2fnxprjt05glZ home]# 
> [root@iZbp1d3xjg2fnxprjt05glZ home]# docker ps
> CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS                                   NAMES
> 7d066d1bf6ed   centos    "/bin/bash"              17 seconds ago   Up 15 seconds                                           beautiful_moser
> af56584fb4a0   nginx     "/docker-entrypoint.…"   10 hours ago     Up 10 hours     0.0.0.0:5011->80/tcp, :::5011->80/tcp   beautiful_bohr
> [root@iZbp1d3xjg2fnxprjt05glZ home]# docker inspect 7d066d1bf6ed  
> [
>     {
>         "Id": "7d066d1bf6edaf8989288892b88b832480bae7d4d0eac5884c427f81b66432be",
>         "Created": "2022-10-05T03:19:33.847283876Z",
>         "Path": "/bin/bash",
>         "Args": [],
>         "State": {
>             "Status": "running",
>             "Running": true,
>             "Paused": false,
>             "Restarting": false,
>             "OOMKilled": false,
>             "Dead": false,
>             "Pid": 30272,
>             "ExitCode": 0,
>             "Error": "",
>             "StartedAt": "2022-10-05T03:19:34.224852399Z",
>             "FinishedAt": "0001-01-01T00:00:00Z"
>         },
>         "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
>         "ResolvConfPath": "/var/lib/docker/containers/7d066d1bf6edaf8989288892b88b832480bae7d4d0eac5884c427f81b66432be/resolv.conf",
>         "HostnamePath": "/var/lib/docker/containers/7d066d1bf6edaf8989288892b88b832480bae7d4d0eac5884c427f81b66432be/hostname",
>         "HostsPath": "/var/lib/docker/containers/7d066d1bf6edaf8989288892b88b832480bae7d4d0eac5884c427f81b66432be/hosts",
>         "LogPath": "/var/lib/docker/containers/7d066d1bf6edaf8989288892b88b832480bae7d4d0eac5884c427f81b66432be/7d066d1bf6edaf8989288892b88b832480bae7d4d0eac5884c427f81b66432be-json.log",
>         "Name": "/beautiful_moser",
>         "RestartCount": 0,
>         "Driver": "overlay2",
>         "Platform": "linux",
>         "MountLabel": "",
>         "ProcessLabel": "",
>         "AppArmorProfile": "",
>         "ExecIDs": null,
>         "HostConfig": {
>             "Binds": [
>                 "/home/xiaoqi_dir:/home"
>             ],
>             "ContainerIDFile": "",
>             "LogConfig": {
>                 "Type": "json-file",
>                 "Config": {}
>             },
>             "NetworkMode": "default",
>             "PortBindings": {},
>             "RestartPolicy": {
>                 "Name": "no",
>                 "MaximumRetryCount": 0
>             },
>             "AutoRemove": false,
>             "VolumeDriver": "",
>             "VolumesFrom": null,
>             "CapAdd": null,
>             "CapDrop": null,
>             "CgroupnsMode": "host",
>             "Dns": [],
>             "DnsOptions": [],
>             "DnsSearch": [],
>             "ExtraHosts": null,
>             "GroupAdd": null,
>             "IpcMode": "private",
>             "Cgroup": "",
>             "Links": null,
>             "OomScoreAdj": 0,
>             "PidMode": "",
>             "Privileged": false,
>             "PublishAllPorts": false,
>             "ReadonlyRootfs": false,
>             "SecurityOpt": null,
>             "UTSMode": "",
>             "UsernsMode": "",
>             "ShmSize": 67108864,
>             "Runtime": "runc",
>             "ConsoleSize": [
>                 0,
>                 0
>             ],
>             "Isolation": "",
>             "CpuShares": 0,
>             "Memory": 0,
>             "NanoCpus": 0,
>             "CgroupParent": "",
>             "BlkioWeight": 0,
>             "BlkioWeightDevice": [],
>             "BlkioDeviceReadBps": null,
>             "BlkioDeviceWriteBps": null,
>             "BlkioDeviceReadIOps": null,
>             "BlkioDeviceWriteIOps": null,
>             "CpuPeriod": 0,
>             "CpuQuota": 0,
>             "CpuRealtimePeriod": 0,
>             "CpuRealtimeRuntime": 0,
>             "CpusetCpus": "",
>             "CpusetMems": "",
>             "Devices": [],
>             "DeviceCgroupRules": null,
>             "DeviceRequests": null,
>             "KernelMemory": 0,
>             "KernelMemoryTCP": 0,
>             "MemoryReservation": 0,
>             "MemorySwap": 0,
>             "MemorySwappiness": null,
>             "OomKillDisable": false,
>             "PidsLimit": null,
>             "Ulimits": null,
>             "CpuCount": 0,
>             "CpuPercent": 0,
>             "IOMaximumIOps": 0,
>             "IOMaximumBandwidth": 0,
>             "MaskedPaths": [
>                 "/proc/asound",
>                 "/proc/acpi",
>                 "/proc/kcore",
>                 "/proc/keys",
>                 "/proc/latency_stats",
>                 "/proc/timer_list",
>                 "/proc/timer_stats",
>                 "/proc/sched_debug",
>                 "/proc/scsi",
>                 "/sys/firmware"
>             ],
>             "ReadonlyPaths": [
>                 "/proc/bus",
>                 "/proc/fs",
>                 "/proc/irq",
>                 "/proc/sys",
>                 "/proc/sysrq-trigger"
>             ]
>         },
>         "GraphDriver": {
>             "Data": {
>                 "LowerDir": "/var/lib/docker/overlay2/311f5fef889cce0dbe5a4e1f661b6194accba7dc55a7243d44e2b6538fc29dd7-init/diff:/var/lib/docker/overlay2/030af70baf09379ab48e30544e0d56ab1b4a047173c488b463a379bce0a4f081/diff",
>                 "MergedDir": "/var/lib/docker/overlay2/311f5fef889cce0dbe5a4e1f661b6194accba7dc55a7243d44e2b6538fc29dd7/merged",
>                 "UpperDir": "/var/lib/docker/overlay2/311f5fef889cce0dbe5a4e1f661b6194accba7dc55a7243d44e2b6538fc29dd7/diff",
>                 "WorkDir": "/var/lib/docker/overlay2/311f5fef889cce0dbe5a4e1f661b6194accba7dc55a7243d44e2b6538fc29dd7/work"
>             },
>             "Name": "overlay2"
>         },
>         "Mounts": [       #这块 就是挂在的信息
>             {
>                 "Type": "bind",
>                 "Source": "/home/xiaoqi_dir",
>                 "Destination": "/home",
>                 "Mode": "",
>                 "RW": true,
>                 "Propagation": "rprivate"
>             }
>         ],
>         "Config": {
>             "Hostname": "7d066d1bf6ed",
>             "Domainname": "",
>             "User": "",
>             "AttachStdin": true,
>             "AttachStdout": true,
>             "AttachStderr": true,
>             "Tty": true,
>             "OpenStdin": true,
>             "StdinOnce": true,
>             "Env": [
>                 "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
>             ],
>             "Cmd": [
>                 "/bin/bash"
>             ],
>             "Image": "centos",
>             "Volumes": null,
>             "WorkingDir": "",
>             "Entrypoint": null,
>             "OnBuild": null,
>             "Labels": {
>                 "org.label-schema.build-date": "20210915",
>                 "org.label-schema.license": "GPLv2",
>                 "org.label-schema.name": "CentOS Base Image",
>                 "org.label-schema.schema-version": "1.0",
>                 "org.label-schema.vendor": "CentOS"
>             }
>         },
>         "NetworkSettings": {
>             "Bridge": "",
>             "SandboxID": "c9509fcde51845a61051d9c9cf7350c72dcb2a19f0c7ffaf7dc082cd9a01701f",
>             "HairpinMode": false,
>             "LinkLocalIPv6Address": "",
>             "LinkLocalIPv6PrefixLen": 0,
>             "Ports": {},
>             "SandboxKey": "/var/run/docker/netns/c9509fcde518",
>             "SecondaryIPAddresses": null,
>             "SecondaryIPv6Addresses": null,
>             "EndpointID": "be491bed18abfbe6bdfd35ebacc1ada1e51ef587fe3f34a8126d5bdcc4465077",
>             "Gateway": "172.18.0.1",
>             "GlobalIPv6Address": "",
>             "GlobalIPv6PrefixLen": 0,
>             "IPAddress": "172.18.0.3",
>             "IPPrefixLen": 16,
>             "IPv6Gateway": "",
>             "MacAddress": "02:42:ac:12:00:03",
>             "Networks": {
>                 "bridge": {
>                     "IPAMConfig": null,
>                     "Links": null,
>                     "Aliases": null,
>                     "NetworkID": "9f8b1799a7346caa299f944cf527c761e626dfa02beba1cd616fcd25342fd3c0",
>                     "EndpointID": "be491bed18abfbe6bdfd35ebacc1ada1e51ef587fe3f34a8126d5bdcc4465077",
>                     "Gateway": "172.18.0.1",
>                     "IPAddress": "172.18.0.3",
>                     "IPPrefixLen": 16,
>                     "IPv6Gateway": "",
>                     "GlobalIPv6Address": "",
>                     "GlobalIPv6PrefixLen": 0,
>                     "MacAddress": "02:42:ac:12:00:03",
>                     "DriverOpts": null
>                 }
>             }
>         }
>     }
> ]
> 
> 
> ```
>

## mysql 部署

> https://hub.docker.com/_/mysql  #参考地址

#指定版本下载mysql

```shell
docker pull mysql:5.7 
#启动
-v 数据卷绑定
-p 端口映射
-d 后台运行
-e 环境配置
--name 设置名字
docker run -it --name xiaoqi_mysql -e MYSQL_ROOT_PASSWORD=123456 -v /home/xiaoqi_mysql/conf:/etc/mysql/conf.d  -v /home/xiaoqi_mysql/data:/var/lib/mysql -p 5012:3306 -d mysql:5.7
```

#官方直接下载启动

> ```console
> docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag
> ```

## 具名挂载 和匿名挂载

```shell
-v name:容器内路径 	#居名挂载
docker run --name my-nginx01 -v xqname:/etc/nginx/ -d nginx
-v 容器内路径		#匿名挂载
docker run --name my-nginx -v /etc/nginx/ -d nginx
#默认存储路径 
# ro rw 设置权限
/etc/nginx:ro

/var/lib/docker/volumes/
-v 宿主机路径:容器内路径 #指定路径挂载
```

卷共享

>  docker run --name my-nginx01 -v xqname:/etc/nginx/ -d nginx	
>
>  docker run --name my-nginx02  -d nginx --volumes-from my-nginx01





## Doeckerfile

![image-20221005162241670](C:\Users\qishou\AppData\Roaming\Typora\typora-user-images\image-20221005162241670.png)

#### dockerfile 指令

![image-20221005171730548](C:\Users\qishou\AppData\Roaming\Typora\typora-user-images\image-20221005171730548.png)

#### asp.net core 发布成docker镜像

```shell
1、将项目发布到指定文件下
2、复制Dockefile 文件 并修改内容如下
FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
COPY . .
ENTRYPOINT ["dotnet", "DockerDemo.dll"]
3、查看生成好得镜像 docker image ls
PS D:\Code\C#\xx> docker image ls
REPOSITORY                        TAG       IMAGE ID       CREATED          SIZE
dockerdemo                        0.1       87ff54bd8a52   7 minutes ago    212MB
dockerdemo                        latest    87ff54bd8a52   7 minutes ago    212MB
<none>                            <none>    3887d38bf080   10 minutes ago   212MB
dockerdemo                        dev       5b130ccb74ab   11 hours ago     208MB
mcr.microsoft.com/dotnet/aspnet   6.0       62535cb3fa6e   2 days ago       208MB
4、运行镜像
docker run -dit -p 8000:80 87ff54bd8a52
```



## Docker 网络

### 默认网络测试

#查看Docker 得网络

> [root@iZbp1d3xjg2fnxprjt05glZ ~]# docker network ls
> NETWORK ID     NAME      DRIVER    SCOPE
> 9f8b1799a734   bridge    bridge    local
> 94f9d0724efb   host      host      local
> 655a8bcafc9a   none      null      local

 #运行 两个 alpine

```shell
 docker run -dit --name alpine1 alpine ash
 docker run -dit --name alpine2 alpine ash
```

 进入容器内部 ip addr 查看ip 地址

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker attach alpine2
/ # ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
36: eth0@if37: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP 
    link/ether 02:42:ac:12:00:03 brd ff:ff:ff:ff:ff:ff
    inet 172.18.0.3/16 brd 172.18.255.255 scope global eth0
       valid_lft forever preferred_lft forever

```

>  #ping 测试 
>
> [root@iZbp1d3xjg2fnxprjt05glZ ~]# docker attach alpine1
> / # ping -c 2 172.18.0.3
> PING 172.18.0.3 (172.18.0.3): 56 data bytes
> 64 bytes from 172.18.0.3: seq=0 ttl=64 time=0.124 ms
> 64 bytes from 172.18.0.3: seq=1 ttl=64 time=0.085 ms
>
> --- 172.18.0.3 ping statistics ---

直接通过ip 可以测试通过 

 1、通过容器名测试

```shell
/ # ping -c 2 alpine2
ping: bad address 'alpine2'
失败
```

这个问题 官方建议 不要用默认得 bridge network 要用自定义得 network

移除 刚才得两个容器

```shell
docker container stop alpine1 alpine2
docker container rm alpine1 alpine2
```

### 自定义网络测试

1、创建网络并查看信息

```shell
root@iZbp1d3xjg2fnxprjt05glZ ~]# docker network create --driver bridge alpine-net
09f00b029889d491dd5fe2a2d5db8473d45f26d9d0246c1e82938cc56d0d3b18
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker network ls
NETWORK ID     NAME         DRIVER    SCOPE
09f00b029889   alpine-net   bridge    local
9f8b1799a734   bridge       bridge    local
94f9d0724efb   host         host      local
655a8bcafc9a   none         null      local
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker network inspect alpine-net
[
    {
        "Name": "alpine-net",
        "Id": "09f00b029889d491dd5fe2a2d5db8473d45f26d9d0246c1e82938cc56d0d3b18",
        "Created": "2022-10-07T00:28:24.048259931+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
[root@iZbp1d3xjg2fnxprjt05glZ ~]# 

```

2 、启动测试容器  

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker run -dit --name alpine1 --network alpine-net alpine ash
818a400f4adefa49aa37f8ec456ce5c43ef4c658f247468f44134953cfe3b9c0
[root@iZbp1d3xjg2fnxprjt05glZ ~]#  docker run -dit --name alpine2 --network alpine-net alpine ash
b141922a613e996484dea8b33b82ca98ae9f7ca3f6d3b69eb222a29508cab00a
[root@iZbp1d3xjg2fnxprjt05glZ ~]#  docker run -dit --name alpine3 alpine ash
4ccbcccbebebed57546952a9669d54bea6b43859aa52fc7ce297f28210a35a1c
[root@iZbp1d3xjg2fnxprjt05glZ ~]#  docker run -dit --name alpine4 --network alpine-net alpine ash
81f1ad808419cd32612532be382e186fde50802e0fa36047e857d2992d3e1bbd
[root@iZbp1d3xjg2fnxprjt05glZ ~]#  docker network connect bridge alpine4
[root@iZbp1d3xjg2fnxprjt05glZ ~]# 
 # --network 指定所用得网络  如果不指定 则使用默认得 bridge
 
 # network connect 可以将不在同一个网络得容器连接起来
```



3 、查看默认网络 和自定义网络

3.1 查看自定义网络

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker network inspect alpine-net
[
    {
        "Name": "alpine-net",
        "Id": "09f00b029889d491dd5fe2a2d5db8473d45f26d9d0246c1e82938cc56d0d3b18",
        "Created": "2022-10-07T00:28:24.048259931+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "172.19.0.0/16",
                    "Gateway": "172.19.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "818a400f4adefa49aa37f8ec456ce5c43ef4c658f247468f44134953cfe3b9c0": {
                "Name": "alpine1",
                "EndpointID": "67e8e9f5672fd5b106b5d1fd06c5cc40c679e445a8c5936eccbb9f247549f57c",
                "MacAddress": "02:42:ac:13:00:02",
                "IPv4Address": "172.19.0.2/16",
                "IPv6Address": ""
            },
            "81f1ad808419cd32612532be382e186fde50802e0fa36047e857d2992d3e1bbd": {
                "Name": "alpine4",
                "EndpointID": "16892c442bf7811331c8ad00abeac6b5bebf8fe7833c09b7d93239e2909b8b89",
                "MacAddress": "02:42:ac:13:00:04",
                "IPv4Address": "172.19.0.4/16",
                "IPv6Address": ""
            },
            "b141922a613e996484dea8b33b82ca98ae9f7ca3f6d3b69eb222a29508cab00a": {
                "Name": "alpine2",
                "EndpointID": "40b355704c5e065267949c7e6d1e6936c4364d08b96560b5c15d0fd4509286e8",
                "MacAddress": "02:42:ac:13:00:03",
                "IPv4Address": "172.19.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {},
        "Labels": {}
    }
]

```

  可以看到 比原来给多了三个 alpine1 alpine2 alpine4

这三个网络网段一样，不近可以通过ip 地址互通 也可以通过容器名字胡同

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker attach alpine1
/ # ping -c 2 alpine2
PING alpine2 (172.19.0.3): 56 data bytes
64 bytes from 172.19.0.3: seq=0 ttl=64 time=0.096 ms
64 bytes from 172.19.0.3: seq=1 ttl=64 time=0.100 ms

--- alpine2 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.096/0.098/0.100 ms
/ # 

```

alpine1  不能通过ip 和容器名 连接alpine3 因为两个不在同一网段

apline4 可以连接alpine4 是因为 alpine4 被连接到默认网关

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker attach alpine1
/ # ping -c 2 alpine4
PING alpine4 (172.19.0.4): 56 data bytes
64 bytes from 172.19.0.4: seq=0 ttl=64 time=0.093 ms
64 bytes from 172.19.0.4: seq=1 ttl=64 time=0.090 ms

--- alpine4 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
```

3.2 查看默认网关

```shell
[root@iZbp1d3xjg2fnxprjt05glZ ~]# docker network inspect bridge
[
    {
        "Name": "bridge",
        "Id": "9f8b1799a7346caa299f944cf527c761e626dfa02beba1cd616fcd25342fd3c0",
        "Created": "2022-10-04T20:13:30.44840707+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": null,
            "Config": [
                {
                    "Subnet": "172.18.0.0/16",
                    "Gateway": "172.18.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {
            "4ccbcccbebebed57546952a9669d54bea6b43859aa52fc7ce297f28210a35a1c": {
                "Name": "alpine3",
                "EndpointID": "fb8fea2100a8d4c8e78e2577aaa89e91548aa4c02d57b0e4ce7090f5a9a50c9b",
                "MacAddress": "02:42:ac:12:00:02",
                "IPv4Address": "172.18.0.2/16",
                "IPv6Address": ""
            },
            "81f1ad808419cd32612532be382e186fde50802e0fa36047e857d2992d3e1bbd": {
                "Name": "alpine4",
                "EndpointID": "e627d1aec07fd1b32a53e53637ac2bb1d02343a62ed09925719231672310c4af",
                "MacAddress": "02:42:ac:12:00:03",
                "IPv4Address": "172.18.0.3/16",
                "IPv6Address": ""
            }
        },
        "Options": {
            "com.docker.network.bridge.default_bridge": "true",
            "com.docker.network.bridge.enable_icc": "true",
            "com.docker.network.bridge.enable_ip_masquerade": "true",
            "com.docker.network.bridge.host_binding_ipv4": "0.0.0.0",
            "com.docker.network.bridge.name": "docker0",
            "com.docker.network.driver.mtu": "1500"
        },
        "Labels": {}
    }
]

```

可以看到 alpine4  拥有      "IPv4Address": "172.19.0.4/16" 和 IPv4Address": "172.18.0.3/16", 两个ip 地址 所以 两个都可以访问

在容器 alpine4 里 网络测试 可以 通过ip 访问所有得容器 也可以通过容器名 访问alpine1 和2 但是不能访问3 因为3得网段 只存在默认得docker0 上 docker0 是不支持容器名访问得

测试ping alpine3

```shell
/ # ping -c 2 172.18.0.2
PING 172.18.0.2 (172.18.0.2): 56 data bytes
64 bytes from 172.18.0.2: seq=0 ttl=64 time=0.112 ms
64 bytes from 172.18.0.2: seq=1 ttl=64 time=0.101 ms

--- 172.18.0.2 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.101/0.106/0.112 ms
/ # ping -c 2 alpine3
ping: bad address 'alpine3'
/ # 

```

测试ping alpine1

```shell
/ # ping -c 2 172.19.0.2
PING 172.19.0.2 (172.19.0.2): 56 data bytes
64 bytes from 172.19.0.2: seq=0 ttl=64 time=0.087 ms
64 bytes from 172.19.0.2: seq=1 ttl=64 time=0.096 ms

--- 172.19.0.2 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.087/0.091/0.096 ms
/ # ping -c 2 alpine1
PING alpine1 (172.19.0.2): 56 data bytes
64 bytes from 172.19.0.2: seq=0 ttl=64 time=0.058 ms
64 bytes from 172.19.0.2: seq=1 ttl=64 time=0.101 ms

--- alpine1 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.058/0.079/0.101 ms
/ # 

```

移除测试得容器 和自定义网络

```shell
docker container stop alpine1 alpine2 alpine3 alpine4
docker container rm alpine1 alpine2 alpine3 alpine4
docker network rm alpine-net
```





## Docker 导出 导入

对容器导出导入

```shell
1、导出
docker export red_panda > latest.tar
docker export --output="latest.tar" red_panda
2、导入
docker import /path/to/exampleimage.tgz
```

对镜像导入导出

```shell
1、保存到指定文件下
docker save -o D:\Code\C#\xx\dockerdemo.tar dockerdemo:0.1
2、复制到服务器 load 到docker 中
[root@iZbp1d3xjg2fnxprjt05glZ home]# ls
dockerdemo.tar  downloads  mysql  test.txt  xiaoqi_dir2  xiaoqi_mysql  xiaoqi.txt
[root@iZbp1d3xjg2fnxprjt05glZ home]# docker load -i dockerdemo.tar
fe7b1e9bf792: Loading layer [==================================================>]  84.01MB/84.01MB
86febf79b6c0: Loading layer [==================================================>]  36.52MB/36.52MB
8d3c7c2d0b0f: Loading layer [==================================================>]  70.75MB/70.75MB
0aad7b3e5401: Loading layer [==================================================>]   2.56kB/2.56kB
82edbe9d7b93: Loading layer [==================================================>]  20.36MB/20.36MB
846a78100f8f: Loading layer [==================================================>]  2.048kB/2.048kB
2a93ca962e8d: Loading layer [==================================================>]  4.195MB/4.195MB
Loaded image: dockerdemo:0.1
3、查看镜像
[root@iZbp1d3xjg2fnxprjt05glZ home]# docker image ls
REPOSITORY    TAG       IMAGE ID       CREATED          SIZE
dockerdemo    0.1       87ff54bd8a52   37 minutes ago   212MB
4、运行
[root@iZbp1d3xjg2fnxprjt05glZ home]# docker run -dit -p 5015:80 dockerdemo:0.1
6c58e8892fe09aad1ec91e9d33d6368d9bff5ff7224b0c27d4cae944fc0eea02

```

## Docker Compose