# 搭建私有仓库 docker registry

1. 拉取镜像到本地
docker pull registry

2. 运行registry镜像
docker run -d -p 5000:5000 -v /Users/mac/mian/docker_registry:/var/lib/registry registry
-d后台运行
-p指定端口
-v把registry的镜像路径/var/lib/registry映射到本机的/Users/mac/mian/docker_registry
registry 是镜像

3. 查看registry是否成功启动且可用
$ docker ps -a
如果本机有图形界面，浏览器中访问 http://127.0.0.1:5000/v2/ 可以看到返回一个 {}
curl -XGET http://127.0.0.1:5000/v2/_catalog

4. 在docker daemon 里添加仓库 
```
{
"insecure-registries" : [
"127.0.0.1:5000"
],
"debug" : true,
"experimental" : true,
"registry-mirrors" : [
"http://hub-mirror.c.163.com"
]
}

```
5. push 镜像到registry
docker tag xxx 127.0.0.1:5000/xxx:v1
docker push 127.0.0.1:5000/xxx:v1

6. 查看仓库中的镜像
在浏览器访问：http://127.0.0.1:5000/v2/_catalog

7. 从私有仓库中拉取镜像
docker pull 127.0.0.1:5000/xxx:v1
