# docker
 
- [创建镜像]()
```
docker build -t rc-image-guoling-consumer:20171121 .
```
- [创建并运行容器(job/consumer)]()
```
docker run -i -t --name rc-guoling-consumer rc-image-guoling-consumer:20171121 /bin/bash
```
- [创建并运行容器(接口)]()
```
docker run -i -t -p 6006:6006 --name rc-guoling-kdxy_interface_service rc-image-guoling-kdxy_interface_service:20171123 /bin/bash
```
- [查看容器是否运行]()
```
docker ps -a| grep guoling
```
- [进入容器]()
```
docker exec -it 容器ID /bin/bash
```
- [nohup手动启项目]()
```
nohup python Main.py > nohup.out 2>&1 &
```
- [关闭容器]()
```
docker stop 容器ID
```
- [开启容器]()
```
docker start 容器ID
```
- [删除容器]()
```
docker rm 容器ID
```
- [强制删除镜像]()
```
 docker rmi -f 9503b09c1a70   
```
- [删除镜像]()
```
docker rmi 镜像ID
```
- [删除虚拟环境]()
```
删除：rmvirtualenv [虚拟环境名称]
```
- [进入虚拟环境]()
```
workon MiscS_Bp
```
- [退出虚拟环境]()
```
deactivate
```
- [pip版本升级]()
```
python -m pip install -U pip
```
- [查看集群服务]()
```
docker service ls
```
- [关闭整个集群服务]()
```
docker rm  ls  tyxb_prod-lvj-feedback-consumer
```
- [查看xx文件是否启动]()
```
ps -ef | grep Main | grep -v grep
```
- [杀死进程]()
```
在上面基础上 kill -9 进程号码
```
- []()
```
```
- []()
```
```
- []()
```
```
- []()
```
```
- []()
```
```
- []()
```
```
- []()
```
```
- []()
```
```
