# Admin-Shrio-Restful 
- 提供admin页面可配置式的,动态的 ```restful api``` 安全管理支持  
- 数据传输动态秘钥加密,```jwt```过期刷新,用户操作监控等 加固应用安全  

## 使用和一些约定   
--------

- 您使用此项目在后端开发好```api```后,需要在前端页面 资源配置->API管理 新增基于```ant```匹配风格的```api```(约定没有配置的api没有保护)
- ```eg:``` 获取角色关联的对应用户列表 ```rest-url```为 ```/role/user/{roleId}/{currentPage}/{pageSize}```访问方式为```GET```, 您需要在页面新增```api:``` ```/role/user/*/*/*``` ```GET```方式
- 自定义```url```匹配链约定为 ```url=``` ```url+"=="+httpMethod```
- 页面添加了```api```后,您需要在 资源配置->角色管理 配置您想要授权角色的API,菜单,关联用户等资源(约定授权给```auth_anon```角色的```api```可以被所有人访问,注意没有授权给任何角色的api是可以被任何人访问的)
- 授权菜单在第一次登录时已经获取存储到```sessionStorage```防止重复获取,您授权变更菜单之后想要看的效果需要关闭页面重新打开(或者清除```sessionStorage```之后会自动获取授权菜单)
- have fun  

## 项目的基础框架设计：  

总的长这样：  

![image1](/image/image1.PNG)  

<br>

#### 前端usthe  

基于```angular5 + angular-cli + typeScript + rxjs + bootstrap + adminLTE```,践行angular最佳实践。  
过程中node,webpack等有用到过,但我不熟。。。

#### 后端Admin-Shiro-restful  

基于```springboot + apache shiro + mybatis```框架，restful风格api，自定义状态码，json-web-token，druid数据库连接池，swagger文档生成，redis存储refreshtoken和动态秘钥，maven，MD5单向加密和AES双向等。。。  


## 部署  
--------
1.IDE启动调试  

- fork 项目到自己的仓库(欢迎star^.^)  
- clone 项目到本地 git clone https://gitee.com/yourName/Admin-Shiro-restful.git
- 用idea导入
- 更改开发环境mysql数据库和redis地址(前提安装数据库并导入usthe.sql创建数据库usthe)
- 运行Admin-Shiro-restfulApplication
- Admin-Shiro-restful就可以提供api了 http://localhost:8080
- 推荐使用postman进行api调试

2.docker本地启动  

- fork 项目到自己的仓库(欢迎star^.^)  
- clone 项目到本地 git clone https://gitee.com/yourName/Admin-Shiro-restful.git
- 更改生产环境mysql数据库和redis地址(前提安装数据库并导入usthe.sql创建数据库usthe)
- 前提已经存在maven环境,docker环境([docker常用看这里](https://segmentfault.com/a/1190000013088818))
- mvn clean install -Dmaven.test.skip=true打出jar包
- 在项目目录下 docker build -t Admin-Shiro-restful:1.0 . 
- docker images看是否生成镜像成功
- 运行 docker run -d -p 8080:8080 --name haiGirl Admin-Shiro-restful:1.0
- docker ps 就可以看见您的haiGirl了
- Admin-Shiro-restful就可以提供api了 http://localhost:8080

3.jenkins+docker持续集成持续部署CICD  

- fork 项目到自己的仓库(欢迎star^.^)  
- clone 项目到本地 git clone https://gitee.com/yourName/Admin-Shiro-restful.git
- 更改生产和开发环境mysql数据库和redis地址(前提安装数据库并导入usthe.sql创建数据库usthe)
- 搭建CICD环境有点繁琐，[看这里最下面](https://segmentfault.com/a/1190000013088818)
- 参照搭建完成后,Admin-Shiro-restful对应的jenkins下运行shell:
````
#!/bin/bash

#build in jenkins sh

#docker docker hub仓库地址,之后把生成的镜像上传到  registry or docker hub
REGISTRY_URL=127.0.0.1:5000
#docker login --username tomsun28 --password xxxx

#根据时间生成版本号
TAG=$REGISTRY_URL/$JOB_NAME:`date +%y%m%d-%H-%M`

#使用maven 镜像进行编译 打包出 jar 文件
docker run --rm --name mvn -v /opt/dockerWorkspace/maven:/root/.m2 \
-v /opt/dockerWorkspace/jenkins_home/workspace/$JOB_NAME:/usr/src/mvn -w /usr/src/mvn/ \
tomsun28/maven:1.0 mvn clean install -Dmaven.test.skip=true

#使用放在项目下面的Dockerfile打包生成镜像
docker build -t $TAG $WORKSPACE/.

docker push $TAG
docker rmi $TAG

#判断之前运行的容器是否还在，在就删除
if docker ps -a | grep -i $JOB_NAME;then
docker rm -f $JOB_NAME
fi

#用最新版本的镜像运行容器

docker run -d -p 8085:8080 --name $JOB_NAME -v /opt/dockerWorkspace/tomcat/$JOB_NAME/logs:/opt/tomcat/logs $TAG


````        

。。。。。持续同步更新。。。。

======================================

欢迎一起完善哦^^  

<br>
<br>

### 效果展示  

![image4](/image/image4.PNG)   

![image5](/image/image5.PNG)   

![image6](/image/image6.PNG)   

![image7](/image/image7.PNG)   


<br>
<br>
<br># admin-Shrio-Restful
