# GJSDK

GJSDK 用于接入果酱直播间。

功能特性

- [x] 发言(对某人说)
- [x] 送礼物
- [x] 踢人
- [x] 送花


## 内容摘要

- [快速开始](#1-快速开始)
	- [配置工程](#配置工程)
	- [示例代码](#示例代码)
- [版本历史](#版本历史)

## 快速开始

### 配置工程

- 配置你的 Podfile 文件，添加如下配置信息

```
pod 'GJSDK' , :podspec => 'http://static.guojiang.tv/app/ios/gjsdk.podspec'
```

- 安装 CocoaPods 依赖

```
pod update
```
or
```
pod install
```

- Done! 运行你工程的 workspace

### 示例代码

在需要的地方添加

```Objective-C
#import <GJSDK/GJSDK.h>
```

初始化 GJClient

```Objective-C
// 初始化 GJClient 对象
self.client = [GJClient new];
self.client.uid = 0;//登录用户ID
self.client.sid = @"";//会话ID
self.client.appId = @"";//appId
self.client.delegate = self;//接收回调信息

```

进入直播间

```Objective-C
// 初始化 进入直播间
[self.client enterLiveRoom:2919];//进入指定直播间
```

离开直播间
```Objective-C
[self.client quitLiveRoom];
```

发言(对某人说)
```Objective-C
[self.client sendMessage:@"say something"];//发言
//[self.client sendMessage:@"say something to someone" to:6311];//对某人说
```

送花
```Objective-C
[self.client sendFlower];
```

送礼
```Objective-C
[self.client sendGift:12 multiply:2];//送出ID为12的礼物2个
```

踢出某人
```Objective-C
[self.client kickOut:44];//踢出指定uid用户
```


直播间信息获取

```Objective-C
//GJClientDelegate 
- (void)client:(GJClient*)client didConnectError:(NSError*)error;{
  //连接出错，请检查参数sid、uid、appid是否正确，更多信息请查看error
}

- (void)client:(GJClient*)client didRecieveChatMsg:(GJRoomChatMsgModel*)model error:(NSError*)error;{
  //收到聊天信息，error标识了发言失败(可能是发言过于频繁，可能是被禁言，也可能是其他原因，详情请看error)
}

- (void)client:(GJClient*)client didRecieveGiftMsg:(GJRoomGiftMsgModel*)model error:(NSError*)error;{
  //收到礼物，error标识送礼失败(可能是余额不足，可能是送礼参数出错，详情请看error)
}

- (void)client:(GJClient*)client onFirstSendFlower:(GJRoomUserMsgModel*)userMsgModel error:(NSError*)error;{
  //某用户进直播间后首次送花，一般用于公屏显示(xxx送了花🌹)
}

- (void)client:(GJClient*)client didVideoPublish:(BOOL)isPublish {
  //直播开始/结束；此时你可以加载视频或停止视频
}


/**
 禁言消息

 @param client client
 @param model  禁言消息模型
 @param error  当禁言发生错误的信息
 */
- (void)client:(GJClient*)client didRecieveOnBanMsg:(GJOnBanMsgModel*)model error:(NSError*)error;

/**
 被解禁的消息

 @param client client
 @param model  解禁言消息的模型
 @param error  当解除禁言发生错误的信息
 */
- (void)client:(GJClient*)client didRecieveOnUnBanMsg:(GJOnBanMsgModel*)model error:(NSError*)error;

/**
 收到关注的消息

 @param client client
 @param model  收到关注的消息的模型
 */
- (void)client:(GJClient*)client didRecieveAttentionMsg:(GJRoomUserAttentionModel*)model;

/**
 用户分享消息

 @param client client
 @param model  用户分享信息模型
 */
- (void)client:(GJClient*)client didRecieveOnUserShareMsg:(GJRoomUserMsgModel*)model;

/**
 设置管理员消息

 @param client  client
 @param isAdmin 管理员状态 设置管理员(YES) 取消管理员(NO)
 @param uid     用户uid
 @param error   当管理员消息接受错误信息
 */
- (void)client:(GJClient*)client didSetAdmin:(BOOL)isAdmin toUser:(NSInteger)uid error:(NSError*)error;

/**
 踢人消息: 有踢主播和踢用户两种

 @param client client
 @param model  当踢出用户的信息
 */
- (void)client:(GJClient*)client didKickOutUser:(GJOnKickOutMsgModel*)model;

/**
 资金不足消息

 @param client client
 */
- (void)didReceiveBalanceNotEnoughMessageInClient:(GJClient*)client;

/**
 接收到弹幕消息

 @param client         client
 @param danumuMsgModel 弹幕消息模型
 */
- (void)client:(GJClient*)client didRecieveDanumuMsg:(GJDanumuMsgModel*)danumuMsgModel;

/**
 接收到送花消息

 @param client  client
 @param fromUid 送花者的uid
 @param error   当送花消息接收错误信息
 */
- (void)client:(GJClient*)client onSendFlowerFrom:(NSInteger)fromUid error:(NSError*)error;

```



## 版本历史

- 0.1.0
	- 发布 CocoaPods 版本
  - 支持收发消息
