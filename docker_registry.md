# 搭建私有仓库 docker registry

1. 拉取镜像到本地 <br>
docker pull registry

2. 运行registry镜像<br>
docker run -d -p 5000:5000 -v /Users/mac/mian/docker_registry:/var/lib/registry registry<br>
-d后台运行<br>
-p指定端口<br>
-v把registry的镜像路径/var/lib/registry映射到本机的/Users/mac/mian/docker_registry<br>
registry 是镜像<br>

3. 查看registry是否成功启动且可用<br>
$ docker ps -a<br>
如果本机有图形界面，浏览器中访问 http://127.0.0.1:5000/v2/ 可以看到返回一个 {}<br>
curl -XGET http://127.0.0.1:5000/v2/_catalog<br>

4. 在docker daemon 里添加仓库 <br>
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
5. push 镜像到registry<br>
docker tag xxx 127.0.0.1:5000/xxx:v1
docker push 127.0.0.1:5000/xxx:v1<br>

6. 查看仓库中的镜像
在浏览器访问：http://127.0.0.1:5000/v2/_catalog<br>

7. 从私有仓库中拉取镜像<br>
docker pull 127.0.0.1:5000/xxx:v1
