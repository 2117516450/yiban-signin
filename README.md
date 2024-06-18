# 免责声明
## 使用本项目，您必须接受以下内容，否则应当退出此页：

* 此项目运行过程中可能会会收集您的个人信息（用于判断脚本是否能正常工作）
* 仅额外提供给“记性较差忘记签到”的人
* 使用此脚本有可能违反您所在 单位/学校 的规章制度
* 使用此脚本所造成的一切后果您应当明白，包括但不限于：
* 账号被封停
* 您个人被给予相关处罚
> 使用并复制了此脚本，则默认视为您`已经接受`了此免责声明。请仔细阅读

## [脚本支持列表点此](https://github.com/2117516450/yiban-signin/?tab=readme-ov-file#%E5%85%8D%E8%B4%A3%E5%A3%B0%E6%98%8E)
- - - 
## [下载点此](https://github.com/2117516450/yiban-signin/releases)
- - - 
此脚本已开源部分代码及制作方式/思路：
https://www.wcyuns.cn/archives/yiban

将视情开源所有代码
- - - 

# 实现功能

- [x] 易班签到(基于易班校本化的签到都可以使用此脚本)
- [x] 多用户签到
- [x] 获取签到排名
- [x] 自动校正签到时间
- [x] 签到提交
  - [x] 自动获取签到位置
  - [x] 使用绑定的设备签到
  - [x] 自定义位置
  - [X] 图片签到
- [x] 自动更新
- [x] 签到结果微信推送
- [ ] 多线程签到（多用户）
```diff
- 图片签到有校内和校外之分，目前仅支持校外
```
#### 如果您的学校不支持此脚本或在使用中有任何问题，请提交Issues联系我
```mermaid
graph TD;
    易班-->校本化;
    校本化-->晚点签到;
    晚点签到-->到签到时间;
    晚点签到-->未到签到时间;
    到签到时间-->获取签到位置;
    未到签到时间--->等待到达签到时间;
    等待到达签到时间--->到签到时间;
    获取签到位置-->自动获取到的位置;
    获取签到位置-->用户配置的位置;
    自动获取到的位置-->范围内签到;
    用户配置的位置-->范围内签到;
    用户配置的位置-->范围外签到;
    范围内签到-->范围内需要上传图片签到;
    范围内签到-->范围内需要使用绑定设备签到;    
    范围外签到-->范围外需要上传图片签到;
    范围外签到-->范围外需要使用绑定设备签到;
    范围内需要上传图片签到-->成功签到;
    范围内需要使用绑定设备签到-->成功签到;
    范围外需要上传图片签到-->成功签到;
    范围外需要使用绑定设备签到-->成功签到;
    成功签到-->获取签到结果/排名;
    获取签到结果/排名-->通过配置的方式发送给用户;
```
### 脚本基于python，理论上支持各种平台

# 运行环境
## Windows
已封装`exe` 已在`Windows11` `Windows10`上测试通过（理论上全Windows通用）
待Windows脚本完全正常后开始制作linux脚本
## Linux
待制作

# 运行方式
## Windows
双击 `易签.exe` 后，会自动在目录下生成`user_status.json`文件，填写`user_status.json`文件后重新运行程序即可开始签到
## Linux
待制作

# 配置脚本
详细配置详解请到： [json配置](https://github.com/2117516450/yiban-signin/blob/main/json.md)

脚本使用已经尽量简便化

基础使用只需要在配置中填写易班的账号密码

只要正确输入账号密码就可以开始自动签到

详细配置在`user_status.json`文件中（运行exe后自动生成）
如对配置脚本有疑问，请提交Issues联系我

# 用户配置

## 单用户账户配置

```json
    "user": [
        {
            "user": "填写用户名(手机号/学号)",
            "password": "填写密码",
            "PhoneModel": "填写设备型号",
            "PhoneCode": "填写设备唯一识别码",
            "pushplustoken": "这里填pushplus(推送加)的token（如不使用可以不填）"
        }
    ]
```

## 多用户账户配置

```json
    "user": [
        {
            "user": "填写用户名1",
            "password": "填写密码1"
            "PhoneModel": "填写设备型号1",
            "PhoneCode": "填写设备唯一识别码1",
            "pushplustoken": "这里填pushplus(推送加)的token（如不使用可以不填）"
        },
        {
            "user": "填写用户名2",
            "password": "填写密码2"
            "PhoneModel": "填写设备型号2",
            "PhoneCode": "填写设备唯一识别码2",
            "pushplustoken": "这里填pushplus(推送加)的token（如不使用可以不填）"
        }
    ]
```

更多用户以此类推

## 签到位置

```json
    "LngLat": {
        "stat": 0,
        "#stat": "0 自动获取位置 1 使用下面设置的位置",
        "lon": "",
        "#lon": "经度，如：123.123456",
        "lat": "",
        "#lat": "纬度，如：12.123456",
        "Adders": "",
        "#Adders": "地址，如：**省**市"
        "Reason": "",
        "#Reason-说明": "填写使用在范围外签到(图片签到)的原因，如：定位错误",
        "photopath": "",
        "#photopath-说明": "填写使用在范围外签到(图片签到)的原因，如：C:/Users/Administrator/Desktop/签到.jpg"
    }
```

需要修改`lon` `lat`和`Address`

获取签到座标及地址可以参考: 

# 完整配置解析

```json
{
    "#-说明1": "修改本文件必看",
    "#-说明2": "此文件是保存用户登录信息及签到的配置文件",
    "#-说明3": "如不清楚请不要修改 LngLat 下的数据",
    "#-说明5": "除非提示经纬度获取失败,否者不要动LngLat下的数据",
    "#-说明6": "配置文件内所有符号均为英文输入法下符号,如,{}[];输入中文符号会导致配置文件读取失败",
    "#-说明7": "错误修改可能导致程序崩溃",
    "user": [
        {
            "user": "",
            "password": "",
            "PhoneModel": "",
            "PhoneCode": "",
            "pushplustoken": ""
        }
    ],
    "LngLat": {
        "stat": 0,
        "lon": "",
        "lat": "",
        "Adders": "",
        "Reason": "",
        "photopath": "",
        "#stat-说明": "值为0自动获取经纬+随机计算的经纬，为1使用填写的固定经纬度,默认为0",
        "#lon-说明": "填写签到经度,如100.254877123456（保留6位小数）",
        "#lat-说明": "填写签到纬度,如20.123456（保留6位小数）",
        "#Adders-说明": "填写签到地址,如：**省**市**区**镇**学院(**校区)",
        "#Reason-说明": "填写使用在范围外签到(图片签到)的原因",
        "#photopath-说明1": "如果需要上传图片，填写图片的完整路径，路径不要有引号，尽量使用jpg图片，范例如下",
        "#photopath-说明2": "C:/Users/Administrator/Desktop/签到.jpg"
    },
    "reset": 0,
    "#reset-说明": "1为遇到签到错误自动重新运行，2为遇到所有错误都自动重新运行（包括网络错误，与'skip'冲突），0为不自动运行，自动运行有可能导致使用pushplus遭到封号",
    "skip": 0,
    "#skip-说明": "1为遇到运行错误自动跳过，2为遇到所有错误都自动跳过（包括网络错误），0为不自动跳过，使用自动跳过有可能导致漏签",
    "exit": 0,
    "#exit-说明": "1为签到完成后自动退出，0为不自动退出，自动退出方便服务器后台运行签到程序",
    "start": 1,
    "#user-单用户说明": [
        {
            "user": "这里填用户的账号",
            "password": "这里填用户的账户密码",
            "PhoneModel": "这里填用户的设备编号(不填则自动获取)",
            "pushplustoken": "这里填pushplus(推送加)的token（如不使用可以不填）"
        }
    ],
    "#user-多用户说明": [
        {
            "user": "这里填用户1的账号",
            "password": "这里填用户1的账户密码",
            "PhoneModel": "这里填用户1的设备编号(不填则自动获取)",
            "pushplustoken": "这里填pushplus(推送加)的token（如不使用可以不填）"
        },
        {
            "user": "这里填用户2的账号",
            "password": "这里填用户2的账户密码",
            "PhoneModel": "这里填用户2的设备编号(不填则自动获取)",
            "pushplustoken": "这里填pushplus(推送加)的token（如不使用可以不填）"
        },
        "2用户举例，可添加更多用户,完整复制 {}, 内的内容粘贴在后面即可",
        "设备型号推荐自动获取，填写错误会导致签到失败",
        "pushplus(推送加)是集成了微信、短信、邮件、企业微信、HiFlow连接器、钉钉、飞书等渠道的信息推送平台",
        "为使用方便，强烈建议注册并填写获取到的token，可以很方便的获取签到成功状态"
    ],
    "log": "OFF",
    "#log-日志说明": "OFF(关闭)|INFO(信息)|debug(调试)",
    "config_version": "300"
}
```

- - -

# 特别说明
* 第一次使用如提示登录失败需要在同一网络下在手机上将易班重新登录即可
* 同一网络指：共用一个wifi、共用一条宽带
* 如过一段时间出现登录不上，签不了到的情况，请登录手机易班一次即可
* python 脚本完全开源时间待定


# 支持学校列表
```diff
+ 福建林业职业技术学院 晚签
+ 四川工业科技学院 
+ 南京工程学院 早签
+ 四川轻化工大学
+ 台州学院 晚签
+ 三江学院
+ 东莞职业技术学院
+ 海南卫生健康职业学院
+ 福州大学
- 更多学校敬请反馈
```
