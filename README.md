# ELEX中台 iOS SDK.

<p align="center" >
  <img src="./Docs/images/logo.png" alt="Elex" title="Elex">
</p>

![Objective-C](https://img.shields.io/badge/Objective--C-blue.svg?style=flat)
![Swift-5](https://img.shields.io/badge/Swift-5-red.svg?style=flat)
![Platform](https://img.shields.io/badge/platform-iOS-A1A1A1?style=flat)
![Region](https://img.shields.io/badge/region-CN_|_Oversea-green.svg?style=flat)
![version](https://img.shields.io/badge/iOS-12.0-orange.svg?style=flat)

## Requirements
- iOS 12.0+
- Xcode 13.0+
- Swift 5.5+

## Features
|   模块   |
| :-----: |
|   账号   |
|   支付   |
|   埋点   |
|   跳转   |
|   推送   |
|   客服   |

### Account
|     登录方式    |  支持地区  | 最低iOS版本 |
| :------------: | :------: | :--------: |
|  手机号一键登录  |    国内   |   iOS 10   |
| 手机号验证码登录 |    国内   |   iOS 10   |
| 审核账号密码登录 |    国内   |   iOS 10   |
|    自动登录    | 国内/海外 |   iOS 10   |
|   Apple登录   |    海外    |   iOS 10   |
|    游客登录    |    海外    |   iOS 10   |
|    邮箱登录    |    海外    |   iOS 10   |
|  Facebook登录  |    海外    |   iOS 12   |
|   Google登录   |    海外    |   iOS 10   |


### Pay
|     IAP    |  支持地区  | 最低iOS版本 |
| :--------: | :------: | :--------: |
|  StoreKit  |  国内/海外 |   iOS 10   |
| StoreKit2  |  国内/海外 |   iOS 15   |

### Report
|      埋点平台       |  支持地区  | 最低iOS版本 |
|  :------------:   | :------: | :--------: |
|  ElexData自研埋点  |  国内/海外 |   iOS 10   |
|     AppsFlyer     |  国内/海外 |   iOS 10   |
|     Facebook      |    海外   |   iOS 12   |
|     Firebase      |    海外   |   iOS 10   |


## Installation 

EPSDK支持[CocoaPods](https://cocoapods.org)接入

[CocoaPods](http://cocoapods.org) is a dependency manager for Cocoa projects. You can install it with the following command:

```bash
$ gem install cocoapods
```

```ruby
source 'git@git.elex-tech.com:opt-connect/open-ios/elex-sdk-repo.git'
source 'https://cdn.cocoapods.org'
platform :ios, '10.0'

target 'UnityFramework' do
  pod 'EPSDK'
end
```

Then, run the following command:

```bash
$ pod install
```

# Usage
## 配置文件
添加SDKConfig_iOS.json文件

```json
{
  "base": {
    "env": 2,
    "gameId": 1001,
    "secret": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "region": "en",
    "language": "zh-CN",
    "channel": "appstore",
    "pkgId": "elex",
    "userProtocol": "http://www.linekong.com/articles-237-7022.shtml",
    "privacyPolicy": "http://www.linekong.com/articles-237-7022.shtml",
  },
  "apple": {
    "appId": "xxxxxxxxxx"
  }
}

```
- base:
	- env:SDK环境，0:release,1:sandbox,2:dev
	- gameId: SDK分配的appid
	- secret: SDK分配密钥
	- region: 发行地区
	- language:SDK默认语言
	- channel:渠道
	- pkgId:包标识符
	- userProtocol:用户协议页面地址
	- privacyPolicy:隐私协议页面地址
 - apple:
    - appId: app store app ID    
---
## 初始化
导入头文件
```objc
#import <EPSDK/EPSDK.h>
```
初始化方法
```objc
/**
 初始化SDK
 
 开始初始化SDK；首次调用会弹出隐私协议弹框，

 @param delegate sdk回调代理
 @warning 不要在application:didFinishLaunch:中调用；否则弹不出隐私协议弹框；
 */
- (void)startWithDelegate:(id<EPSDKDelegate>)delegate;
```
示例：
```objc
[[EPSDK sharedSDK] startWithDelegate:self];
```
### EPSDK回调代理方法
```objc

/// EPSDK回调游戏的代理；
@protocol EPSDKDelegate <NSObject>
@optional

///-------------------------------
/// @name 账号
///-------------------------------

/** 
 sdk账号退出回调方法
 @param epsdk EPSDK实例
 @param info 附带信息
 */
- (void)epsdk:(EPSDK *)epsdk didLogoutWithInfo:(nullable NSDictionary *)info;

/**
 sdk账号退出回调方法
 @param epsdk EPSDK实例
 */
- (void)epsdkDidLogout:(EPSDK *)epsdk;

///-------------------------------
/// @name 初始化
///-------------------------------

/**
 sdk初始化成功回调
 @param epsdk EPSDK实例
*/
- (void)epsdkDidInitSuccess:(EPSDK *)epsdk;

/**
 sdk初始化失败回调
 @param epsdk EPSDK实例
 @param error 初始化错误信息NSError
*/
- (void)epsdk:(EPSDK *)epsdk initError:(NSError *)error;

@end
```

## 进入游戏[必接]
进入游戏时上报SDK玩家信息；
warning:此接口必须调用，若不调用支付功能无法正常使用
```objc
/**
 进入游戏
 
 进入游戏时调用此方法，将游戏的角色信息传递给SDK；
 @param playerInfo 角色信息
*/
- (void)enterGameWithPlayerInfo:(EPGamePlayerInfo *)playerInfo;
```

```objc
/**
 游戏角色信息类
 */
@interface EPGamePlayerInfo : NSObject

/// 角色ID
@property (copy,   nonatomic) NSString *eppPlayerId;
/// 角色名称
@property (copy,   nonatomic) NSString *eppPlayerName;
/// 游戏服务器ID
@property (copy,   nonatomic) NSString *eppServerId;
/// 角色等级
@property (copy,   nonatomic) NSString *eppLevel;
/// 角色VIP等级
@property (copy,   nonatomic) NSString *eppVipLevel;
/// 角色主城等级
@property (copy,   nonatomic) NSString *eppCastleLevel;
/// 角色创建时间
@property (assign, nonatomic) long long eppCreateTime;

@end
```

## 更新角色信息[必接]
```objc
/**
 更新游戏的角色信息
 
 游戏角色信息更新时调用此方法，将更新后的角色信息传递给SDK；
 @param playerInfo 角色信息
*/
- (void)updateGamePlayerInfo:(EPGamePlayerInfo *)playerInfo;
```
---

## 账号
[账号](./Docs/Account/README.md)

## 防沉迷
[防沉迷](./Docs/AntiAddict/README.md)

## 支付
[支付](./Docs/Pay/README.md)

## 埋点
[埋点](./Docs/Track/README.md)

## 跳转
[跳转](./Docs/Jump/README.md)

## 推送
[推送](./Docs/Push/README.md)

## 客服
[客服](./Docs/CustomService/README.md)

---

## 通用错误码
|  错误码  |       说明       |
| :-----: | :--------------: |
| 10000 | 网络请求出错 |
| 10001 | 系统错误 |
| 10002 | 参数错误(server) |
| 10005 | 未知错误 |
| 10006 | 签名错误 |
| 10007 | 数据库错误 |
| 10008 | Token过期 |
| 10009 | 游戏不存在 |
| 10010 | 用户不存在 |
| 10011 | 渠道不存在 |
| 10012 | 授权过期 |
| 50000 | 未初始化SDK |
| 50001 | Unity Editor不支持（未使用） |
| 50002 | sdk, 国内通行证和渠道（未使用） |
| 50003 | sdk, 国内通行证和渠道 |
| 50004 | SDK未登录 |
| 50005 | 参数错误 |
| 50006 | 服务器响应数据错误 |
| 50007 | 不支持引导模式 |
| 50008 | 加载配置文件失败 |
| 50009 | SDK重复初始化 |


> Note: EPSDK使用cocoapods方式接入，在unity2019之后的版本，unity统一导出UnityFramework.framework,SDK需要接入到framework中，需要在Podfile中修改SDK的target的依赖。

```ruby
post_install do |installer|
    handle_mainTarget('Pods-Unity-iPhone', installer)
end

def find_line_with_start(str, start)
    str.each_line do |line|
        if line.start_with?(start)
            return line
        end
    end
    return nil
end

def handle_mainTarget(name, installer)
    installer.pods_project.targets.each do |target|
        if target.name != name
            next
        end
        target.build_configurations.each do |config|
            xcconfig_path = config.base_configuration_reference.real_path
            xcconfig = File.read(xcconfig_path)
            old_line = find_line_with_start(xcconfig, "OTHER_LDFLAGS")
        
            if old_line == nil
                next
            end
            new_line = "OTHER_LDFLAGS = $(inherited) -ObjC\n"
      
            new_xcconfig = xcconfig.sub(old_line, new_line)
            File.open(xcconfig_path, "w") { |file| file << new_xcconfig }
        end
    end
end
```
