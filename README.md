# 实现消息队列延迟功能
使用`Rabbitmq`的消息队列延迟功能，即采用官方提供的插件 `rabbitmq_delayed_message_exchange`来实现 ,但`Rabbitmq`版本必须是3.5.8以上才支持该插件，否则得用其死信功能来实现。
## 安装 rabbitmq
- 获取镜像
```
#指定版本，该版本包含了web控制页面
docker pull rabbitmq:management
```
- 运行镜像
```
#方式一：默认guest 用户，密码也是 guest
docker run -d --hostname my-rabbit --name rabbit -p 15672:15672 -p 5672:5672 rabbitmq:management

#方式二：设置用户名和密码
docker run -d --hostname my-rabbit --name rabbit -e RABBITMQ_DEFAULT_USER=user -e RABBITMQ_DEFAULT_PASS=password -p 15672:15672 -p 5672:5672 rabbitmq:management
```
- 访问ui界面
```
http://ip:15672/
```
- 安装`rabbitmq_delayed_message_exchange`插件

[下载地址](http://www.rabbitmq.com/community-plugins.html)  
![image](https://s1.ax1x.com/2020/06/15/Np5z1x.png)

下载好之后，解压得到 **.ez 结尾**的插件包，将其复制到`RabbitMQ`安装目录下的 `plugins` 文件夹。

- `docker`安装插件  
1 查看容器实例 `docker ps`

![image](https://s1.ax1x.com/2020/06/15/Npo3qO.png)
2 将插件拷贝到`RabbitMQ plugins`目录下(以实际路径为准)
```
docker cp /root/workspace/rabbitmq_delayed_message_exchange-3.8.0.ez 6722fd94820e:/plugins
```
- 启动插件 
```
sudo docker exec -it 6722  /bin/bash;
rabbitmq-plugins enable rabbitmq_delayed_message_exchange;
```
![image](https://s1.ax1x.com/2020/06/15/Np7Ku6.png)

- 启动项目
 访问 http://localhost:8080/customSend
![image](https://s1.ax1x.com/2020/06/15/NpHCRA.png)
