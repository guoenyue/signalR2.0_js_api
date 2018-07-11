# 聊天系统

- 聊天系统的逻辑整理

流程：

登录-->成功-->成功接口返回当前用户信息（UserInfo）及当前用户联系人列表(ConcatsList[Concaters])-->一对一聊天Message（消息的接收和发送，主动与被动）/联系人的删除或者额外操作-->前端正常的离线退出、前端异常的离线，重连
 |
 -->失败


一个一对多聊天系统必须是有一个用户登录之后才可以完成的操作，当前系统不支持`路人，陌生人`发来消息


按照以上流程所有可能用到的前端钩子函数有：
    1. 连接(登录)成功函数
        /*
            @platform:client
            @params:userId(当前登录者的id),userInfo(登录者的详情信息),concatsList(登录者联系人列表)
            @name:onConnected
        */
        onConnected(userId,userInfo,concatsList)
    2. 连接(登录)失败函数
        /*
            @platform:client
            @params:err(错误信息)
            @name:onFailedConnect
        */
        onFailedConnect(err);
    3. 联系人状态变更通知函数
        /*
            @platform:client
            @params:type(上线还是离线),userinfo(状态变更的用户),time(上线时间)
            @name:onOnlineStatusChange
            注：本函数也可以根据上线下线不同类型拆成两个函数
        */
        onOnlineStatusChange(type,userinfo[,time])
    4. 消息发送状态通知(该函数应配合后端的发消息函数同时使用)
        /*
            @platform:client
            @params:status(状态),msg(信息)
            @name:onAfterPostMessage
        */
        onAfterPostMessage(status,msg)
    5。 接收到新消息通知
        /*
            @platform:client
            @params:fromUserId(来源消息用户id),fromUserName(来源用户名),message(消息)
            @name:onReceivePrivateMessage
        */
        onReceivePrivateMessage(fromUserId,fromUserName,message);


class User{
    constructor(){
        this.name;
        this.avatar;
        this.id;
        this.desc;
        this.onlineStatus;
    }
}


class UserInfo extends User{
    constructor(){
        super();
    }
}

class Concaters extends User{
    constructor(){
        super();
        this.groupsId;
        this.groups;
        [this.sorted]
    }
}

class Group{
    constructor(){
        this.name;
        this.id;
    }
}

class Message {
    constructor(){
        this.msg;
        this.id;
        this.time;
        this.targetId;
        this.createBy;
        this.receiver;
    }
}


class CurConversational{
    constructor(){
        this.id;
        this.name;
        this.avatar;
        this.message;
    }
}

