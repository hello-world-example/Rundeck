# 快速开始



## Docker 方式安装

> - [Docker Configuration Reference](https://docs.rundeck.com/docs/administration/configuration/docker.html)
> - [Using MySQL as a database backend](https://docs.rundeck.com/docs/administration/configuration/database/mysql.html)

```bash
$ docker pull rundeck/rundeck:3.3.8

# 事先创建 rundeck 库
$ docker run -d --name rundeck -p 4440:4440 -e TZ="Asia/Shanghai" \
	-v /private/docker/volumes/rundeck/data:/home/rundeck/server/data \
	-v /private/docker/volumes/rundeck/user-assets:/home/rundeck/user-assets \
	-e RUNDECK_DATABASE_DRIVER=org.mariadb.jdbc.Driver \
	-e RUNDECK_DATABASE_URL="jdbc:mysql:///rundeck?autoReconnect=true&useSSL=false" \
	-e RUNDECK_DATABASE_USERNAME="admin" \
	-e RUNDECK_DATABASE_PASSWORD="admin@123123" \
	rundeck/rundeck:3.3.8

# 观察日志，直到出现： running at http://0.0.0.0:4440/
$ docker logs -f rundeck
	
# 默认密码 admin / admin
$ curl http://127.0.0.1:4440/
```



## 基本概念

> @see https://blog.csdn.net/liumiaocn/article/details/89150310

|    概念    | 说明                                                         |
| :--------: | ------------------------------------------------------------ |
|  Project   | Project 是进行 Job 管理的场所，也是 Rundeck 使用时的具体实例，Rundeck 可以运行多个 Project |
|    Jobs    | Job 是相关操作的步骤与设定选项以及执行 Job 的 Node 所组成，而在实际的场景中，很多运维的例行操作都可以在 Rundeck 中以 job 的方式进行定义。 |
|   Nodes    | 物理机器或者网络可访问的虚拟设备，是 Rundeck 中管理的资源类型，在 Rundeck 中将 Job 和 Node 进行了连接，使得整体可以进行管理。 |
|  Commands  | 相较于 Job，Command 是可以在 Node 上进行单次执行的可执行的命令，通过 Rundeck 在指定的 Node 上进行此命令的执行。 |
| Executions | 运维的操作在 Rundeck 中抽象成 Job 和 Command，其每次执行都类似与实际运维作业中的例行操作或者一次性的手工操作，每次操作在 Rundeck 中就以 Execution 的形式存在，通过对 Execution，可以更好的确认 Job 或者Command 执行的状态/和结果。 |
|  Plugins   | Rundeck 很多功能都是通过插件的方式来进行设计实现的，Rundeck 本身也有很多社区类型或者商业类型的插件提供给用户使用。 |



## 创建 Project

> - [Project Create](https://docs.rundeck.com/docs/administration/projects/project-create.html)
> - [Commands](https://docs.rundeck.com/docs/manual/06-commands.html)



- 访问 127.0.0.1:4440 进入 Rundeck 后，点击 **New Project**
- **Project Name** 输出 *Dev_Learn_Machine*
- 点击 **Create** 创建
- http://127.0.0.1:4440/project/Dev_Learn_Machine/command/run 进入 **Commands** 页面
- 下拉框 **Show all nodes**   >  Search， 展示的 Node 是 Rundeck 自身的节点
- 在 **Enter a command** 中输入 `ls -al`  > 点击运行，查看 Node 上执行命令的结果



## 新增 远程 Node

> - [RESOURCE-XML](https://docs.rundeck.com/docs/manual/document-format-reference/resource-v13.html)

1. 新增 Key Storage 用于存储**登陆远程 Node 的秘钥信息**

   - 右上角菜单 > Key Storage (`/menu/storage`)
   - **Add or Upload a Key** ，这里选择 **Password**
   - 输入： 密码、存储路径、名称
2. 设置项目默认 Executor
   - 进入 Dev_Learn_Machine > Project Settings > **Edit Configuration...** > Default Node Executor
   - 选择 SSH 方式
     - **SSH Password Storage Path**： 选择上一步保存的 密码
     - **SSH Authentication**：password
3. 增加 Node Source
   - 进入 Dev_Learn_Machine > Project Settings > **Edit Nodes...** > Source 
     - /project/Dev_Learn_Machine/nodes/sources#sources
   -  **Add a new Node Source** > 选择 File （该类型可在线编辑，存储在 Rundeck 本地 Node）
   - **Format**： resourcexml
   - **File Path**：/home/rundeck/user-assets/nodes.xml
   - **Generate**：勾选
   - **Writeable**：勾选
4. 编辑 /home/rundeck/user-assets/nodes.xml

   - 打开 Edit Tab（/project/Dev_Learn_Machine/nodes/sources）

   - 内容如下：


```xml
<?xml version="1.0" encoding="UTF-8"?>

<project>
  <node name="learn218" description="学习机218" tags="" hostname="172.16.2.218:2208" osArch="amd64" osFamily="unix" osName="Linux" username="master"/>
  <node name="learn219" description="学习机219" tags="" hostname="172.16.2.219:2208" osArch="amd64" osFamily="unix" osName="Linux" username="master"/>
  <node name="learn220" description="学习机220" tags="" hostname="172.16.2.220:2208" osArch="amd64" osFamily="unix" osName="Linux" username="master"/>
  <node name="learn221" description="学习机221" tags="" hostname="172.16.2.221:2208" osArch="amd64" osFamily="unix" osName="Linux" username="master"/>
  <node name="learn222" description="学习机222" tags="" hostname="172.16.2.222:2208" osArch="amd64" osFamily="unix" osName="Linux" username="master"/>
  <node name="learn223" description="学习机223" tags="" hostname="172.16.2.223:2208" osArch="amd64" osFamily="unix" osName="Linux" username="master"/>
</project>
```



## Read More

- Administration Guide
  - [Installation](https://docs.rundeck.com/docs/administration/install/)

