# Getting Started

## 通用日志字段
### 通用字段
字段 | 类型 |  说明  
-|-|-
event | String | 事件标识，区分不同日志
appId | Number | 应用ID  
userId | Number | 平台用户ID 
roleId | String | 角色ID 
level | String | 等级 
zoneId | String | 区服ID （如无区服概念默认为“1”） 
area | String | 国家/地区(ISO 3166-1 alpha-2，即两位大写英文字母) 
version | String | 游戏版本号 
ip | String | ip地址 
clientVersion | String | 客户端版本号（客户端日志时需要）
logTime| Datetime | 时间，UTC时区 

### 客户端日志专属字段

调用SDK日志上报接口，将自动补全下面参数

字段 | 类型 |  说明  
-|-|-
os | Number | iOS:0, Android:1, 其他：2
osVersion | String | 操作系统版本OS 11.2.2、Android 8.0.0等
deviceId | String | iOS取用户的IDFV，Android取androidID /UUID
deviceADId | String | iOS取用户的IDFA，Android取Google Advertising ID
sdkVersion | String | SDK版本号 |
model | String | 型号如iPhone 8等 |

## 日志列表

### 新手节点日志 
**event: <code>noviceNodeLogs</code>**

由运营/策划同学定义新手节点，进行日志上报分析

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
nodeId | String | 节点ID |

**建议上报时机：**玩家通过节点后上报

### 创角日志 
**event: <code>createRoleLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
roleName | String | 角色名字 |

**建议上报时机：**玩家创角成功后上报

### 登录日志 
**event: <code>loginLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
roleName | String | 角色名字 |
vipLevel | String | VIP等级，没有为空 |

**建议上报时机：**玩家登录成功后上报


### 登出日志 
**event: <code>logoutLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
userInfo | JSONObject | 用户属性信息

userInfo 为用户扩展数据，方便进行用户数据核查

``` javascript
{
    gender: 0 , // 性别
    curDiamonds: 1000, // 钻石数
    curCoins: 1000, // 金币数
    fightPower: 29, // 战力
    ...
}
```

**建议上报时机：**玩家退出游戏后，下次打开app时上报

### 修改昵称日志 
**event: <code>nickNameLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
nickName | String | 修改后的昵称 |
oldNickName | String | 修改前的昵称 |

**建议上报时机：**玩家修改昵称成功后上报

### 用户升级日志 
**event: <code>levelUpLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
prevLevel | String | 升级前的等级 |
level | String | 升级后的等级（已在通用字段中） |

**建议上报时机：**玩家升级后上报

### 用户资源日志 
**event: <code>resLogs</code>**

例：钻石获取途径：签到/内购/广告/邮箱奖励/任务奖励等

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
resType | String | 资源类型（钻石，绑钻，金币，点数等） |
type | Number | 1：获取/2：消耗 |
way | String | 获取/消耗途径 |
count | Number | 消耗/获取数量（消耗为负值） |
curCount | Number | 当前数量 |
prevCount | Number | 变化前数量 |

**建议上报时机：**玩家获取到资源时上报

### 物品道具日志 
**event: <code>itemLogs</code>**

仅需记录<code>核心物品</code>的日志

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
itemId | String | 物品ID |
itemName | String | 物品名称 |
type | Number | 1：获取/2：丢弃/3: 强化/4：升级/5：卖出/6：交易 |
isBind | Boolean | 是否为绑定 |
expiryDate | datetime | 有效期，永久有效物品传null |

**建议上报时机：**玩家物品道具变化时上报

### 任务日志
**event: <code>taskLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
type | Number | 1：接收任务/2：完成任务 |
taskId | String | 任务ID |

**建议上报时机：**玩家接收，完成时分别上报

### 关卡/副本日志
**event: <code>chapterLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
chapterId | String | 关卡/副本ID |
type | Number | 1：开始/2：结束 |

**建议上报时机：**玩家开始，完成时分别上报

### 活动参与日志
**event: <code>activityLogs</code>**

字段 | 类型 |  说明  
-|-|-
通用字段 | - | <a href='#/logs?id=通用字段'>详见通用字段</a> |
type | String | 活动类型，1：每日活动 2：节日活动 3：其他
activityId | String | 活动ID |

**建议上报时机：**玩家成功参与活动时上报