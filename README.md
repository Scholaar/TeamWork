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

### 后端说明文档

1. 提供用户登录、注册接口（登录路径：“/login”，注册路径：“/register”）
2. 提供socket通信接口（接口路径：“/socket/{uersId}” 注：完整路径是ws://domain:8080/socket/{userId}）