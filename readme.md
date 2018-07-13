##  signalR.js API 

- 依赖资源
    1. JQuery
    2. SignalR
    
注：以上资源均可在assets文件夹找到。
有关signalR的文档资料：
    [微软官方](https://docs.microsoft.com/en-us/aspnet/signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client#clientsetup)
    [个人博客](https://www.cnblogs.com/shenba/p/5416328.html)

```javascript
    /*
    signalr的使用呢，按照官网介绍可以分为代理模式（hubs）和非代理模式，当前项目使用代理模式，我们的介绍也都是与代理模式相关的设置,两者部分设置
    并不完全相同，如有需要了解非代理模式的相关设置及功能，请点击微软官方api了解。

    代理模式的客户端方法全部集成在$.connection[hubName].client对象上；
    代理模式的服务端被客户端调用方法全部集成在$.connection[hubName].server对象上。
    实际上这些方法名字都是任意在客户端与服务端命名，通过$.connection[hubName]这个对象的桥接使得这些方法在客户端和服务端都可以被使用。
    所有的定义方法均为jq的defer函数形式，可以链式done/fail。
    所有在服务器端定义的方法类在客户端调用时均需要一一对应
    非代理模式下用on方法接收
    */


    //初始化配置：
    //设置接口地址url,此项目不设置将会使用hubs文件内定义的默认路径`/signalr`
    //在hubs文件中的默认设置（由系统生成，真正的配置由后端控制）signalR.hub = $.hubConnection("/signalr", { useDefaultPath: false });
    //如果需要自定义设置此项目，必须同时在后端也将对应接口地址改成相同的url
    $.connection.hub.url = '<yourbackendurl>';
    //设置后端接口的qs参数，可以接收qs对象，也可以接收qs字符串
    $.connection.hub.qs = 'qs'||{qsKey:"qsVal"};
    //这两个内容是由用户登录后获取到
    var userid,username;

    var systemHub = $.connection.chatHub;

    //启动链接：
    $.connection.hub.start().done(function () {
        systemHub.server.connect(userid, username); // 调用服务端connect方法
    });

    // 连接IM服务器成功
    
    //状态相关
    
        // 主要是更新在线用户
        systemHub.client.onConnected = function (id, userName, allUsers) {};

        //新用户上线
        systemHub.client.onNewUserConnected = function (id, userId, userName, loginTime) {};

        //用户离线
        systemHub.client.onUserDisconnected = function (id, userName) {};

    //消息相关

        // 调用服务端sendPrivateMessage方法来转发消息
        systemHub.server.sendPrivateMessage(targetUserId,message);

        //接收消息
        systemHub.client.receivePrivateMessage = function (fromUserId, userName, message) {};

        //发送消息时，对方已不在线
        systemHub.client.absentSubscriber = function () {};

```