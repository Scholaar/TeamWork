# 团队编程后端

## **还不知道叫什么队**

### 接口文档

1. 提供用户登录、注册接口
2. 提供socket通信接口
   - 用户点击开始游戏时，请求建立socket连接，（后端将此加入匹配列表）
   - 服务器向客户端发送***匹配***消息（每匹配一个，向所有socket后端发送匹配状态信息）
   - 服务器向客户端发送***匹配成功***消息
   - 用户的***坦克移动***时，向服务器发送动作发生请求（后端接收后，向所有的socket连接广播此消息）
   - 用户的***子弹发射***时，向服务器发送动作发生请求（后端接收后，向所有的socket连接广播此消息）
   - 服务器向客户端发送xx***坦克被摧毁***的信息
   - 服务器向客户端发送玩家存活时间***排行榜***，标志游戏结束
   - ……………………

### 后端开发文档

> 本项目后端开发环境：
>
> JDK：temurin18-jdk
>
> Maven：3.9.4（如果没有飞机，最好配置国内镜像源）
>
> IDE：IntelliJ IDEA 2023.2.4 (Ultimate Edition)

1. 提供用户登录、注册接口（登录路径：“/login”，注册路径：“/register”）
2. 提供socket通信接口（接口路径：“/socket/{uersId}” 注：完整路径是ws://domain:8080/socket/{userId}）
   - 客户端发送socket连接建立的请求，后端根据uid分配session，并存储到一个Set集合中。该集合设为static，后续的多个socket管理，就只需维护此集合
   - 需要维护一个waitList的容器，每当远程客户端通过socket发来匹配请求，将该用户加入waitList。
   - 需要创建一个GamePool类，该类用以维护Tank类和Bullet类（可以把该类具象成一个游戏盒子，一个盒子就是一局游戏）。具体实现采用哈希表，将坦克类加入哈希表
     - 当waitList的表容量大于等于20？时，取出前二十，加入GamePool
     - Tank类由用户socket控制。
     - Bullet类的创建由用户socket通知创建。创建后交由GamePool管理，自主运行（异步）
     - Tank类的移动和Bullet类的创建，这两个事件由用户通知服务器，并由服务器向所有在线socket广播此消息
     - GamePool应当实现碰撞检测方法。碰撞发生后，在类中移除相关的Tank实例与Bullet实例，并向所有在线socket广播此消息（分为坦克-坦克碰撞、坦克-子弹碰撞两种）
     - 时间检测？？？
     - ………………

> 0为up, 1为down, 2为left, 3为right
> 速度为x单位/秒