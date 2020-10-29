# 06-APP自动遍历


## 一、遍历测试价值与解决痛点


业务常见问题？

- app 页面信息中字段内容丢失或者数据异常
- 微信分享不可用
- 用户网络慢时发出请求后退出当前页面发生崩溃
- 某些界面在6.0和8.0的系统上操作体验不同
- app某界面崩溃
- app某接口有报错


测试痛点？

- 快速迭代中自动化用例吃力
  - 没有采用PageObject模式导致自动化用例维护成本大
  - 对自动化框架掌握程度不够
- 测试内容太多导致手工测试无法充分覆盖
  - 界面字段正确性:比如股票相关数据变化，共数十个字段
  - 接又正确性:后端接又传输数据的变化和内容
  - 专项测试回归难度大:内存泄漏、健壮性测试、弱网等测试过程太多
  - 回归工作量大:不回归又会漏测


测试手段改进：

- 自动化测试：
  - 覆盖主要业务的right path
  - 缩小规模尽量降低维护成本
- 手工测试：
  - 覆盖新功能测试
  - 探索性测试
- 遍历测试手段
  - code less:用例维护成本降低到最低
  - automate:尽可能的自动化覆盖回归业务


自动遍历测试的需求

- 自动遍历的需求：
  - 可控：可以定义遍历的路径
  - 可定制：可实现自动输入、自动滑动等基础行为
- 结果分析：
  - 点击前后的截图对比
  - 结果数据建模
  - 新老版本UI Diff
  - app结构思维导图展示



## 二、常见遍历测试工具

- monkey：https://developer.android.com/studio/test/monkey
- maxim：https://github.com/zhangzhao4444/Maxim


自动遍历常见工具：

- google android原生monkey、app crawler
- 百度smartmonkey
- 腾讯newmonkey  
- vigossjjj smart_monkey
- macaca的NoSmoke
- 头条的zhangzhao maxim
- seveniruby appcrawler



### monkey工具使用

*Monkey 是一个命令行工具，可以在任何模拟器实例或设备上运行。它会将伪随机用户事件流发送到系统中，从而对您正在开发的应用软件进行压力测试。*

monkey在官网上定义实际上是一个压力测试工具，用于提高app的健壮性。本质是产生伪随机事件与appactivity进行交互。事件本身不可控，可以滑动、点击等等，这也就暴露啦一些问题。



monkey工具的缺点：

- 不支持业务定制
  - 经常点到外的APP
  - 经常点击到注销和退出
  - 无法灵活控制

```
monkey 参数说明
usage: monkey [-p ALLOWED_PACKAGE [-p ALLOWED_PACKAGE] ...]
              [-c MAIN_CATEGORY [-c MAIN_CATEGORY] ...]
              [--ignore-crashes] [--ignore-timeouts]
              [--ignore-security-exceptions]
              [--monitor-native-crashes] [--ignore-native-crashes]
              [--kill-process-after-error] [--hprof]
              [--match-description TEXT]
              [--pct-touch PERCENT] [--pct-motion PERCENT]
              [--pct-trackball PERCENT] [--pct-syskeys PERCENT]
              [--pct-nav PERCENT] [--pct-majornav PERCENT]
              [--pct-appswitch PERCENT] [--pct-flip PERCENT]
              [--pct-anyevent PERCENT] [--pct-pinchzoom PERCENT]
              [--pct-permission PERCENT]
              [--pkg-blacklist-file PACKAGE_BLACKLIST_FILE]
              [--pkg-whitelist-file PACKAGE_WHITELIST_FILE]
              [--wait-dbg] [--dbg-no-events]
              [--setup scriptfile] [-f scriptfile [-f scriptfile] ...]
              [--port port]
              [-s SEED] [-v [-v] ...]
              [--throttle MILLISEC] [--randomize-throttle]
              [--profile-wait MILLISEC]
              [--device-sleep-time MILLISEC]
              [--randomize-script]
              [--script-log]
              [--bugreport]
              [--periodic-bugreport]
              [--permission-target-system]
              COUNT

```

monkey工具实现：
```
gemini:/ $ which monkey
/system/bin/monkey
gemini:/ $ cat /system/bin/monkey
# Script to start "monkey" on the device, which has a very rudimentary
# shell.
#
base=/system
export CLASSPATH=$base/framework/monkey.jar
trap "" HUP
for a in "$@"; do
    echo "  bash arg:" $a
done
exec app_process $base/bin com.android.commands.monkey.Monkey "$@"
```
基本语法：
```
adb shell monkey [options] <event-count>
```

常用命令：
```
$ adb shell monkey -p com.wlqq -v 200   # -p指定app包名，-v是打印日志，200指200个随机事件
$ adb shell monkey -p com.wlqq --pct-touch 50 --pct-motion 50 --throttle 1000  -vv 200 
```

常用参数：
```
- p ：指定包名
- v ：打印log
--pct-touch：调整轻触事件所占百分比
--throttle ： 在事件之间插入固定的延迟时间
-f ：支持脚本
```


### maxim工具使用

一款快速高效且高度可复用的android自动化测试工具 ，官方文档：https://github.com/zhangzhao4444/Maxim，
testerhome社区文档：https://testerhome.com/topics/11719

优势：
1、高速点击，每秒 10-15 action！
2、多平台兼容！ 同时兼容Android 5-9
3、轻量极简！


#### 操作使用步骤
```
git clone https://github.com/zhangzhao4444/Maxim.git
cd Maxim
adb push framework.jar /sdcard
adb push monkey.jar /sdcard
adb shell \
CLASSPATH=/sdcard/monkey.jar:/sdcard/framework.jar \
exec app_process /system/bin tv.panda.test.monkey.Monkey \
-p com.wlqq \
--uiautomatormix \
--running-minutes 60 -v -v

遍历模式：uiautomatormix、uiautomatordfs、uiautomatortroy
```

```
执行结果：
[Maxim] // Monkey is over!
[Maxim] // Sending rotation degree= 0, false
[Maxim] // Unaccounted for: 301689
[Maxim] // Tested activities 14
[Maxim] // 1 - com.wlqq.activity.HomeActivity
[Maxim] // 2 - com.xiwei.logisitcs.websdk.ui.XWWebContentActivity
[Maxim] // 3 - com.ymm.lib.account.LoginVerifyActivity
[Maxim] // 4 - com.xiwei.logisitcs.websdk.ui.WebActivity
[Maxim] // 5 - com.ymm.qiankundai.ui.DebugPanelActivity
[Maxim] // 6 - com.ymm.qiankundai.tools.pluginlist.DebugPluginListActivity
[Maxim] // 7 - com.wlqq.websupport.activity.WebActivity
[Maxim] // 8 - com.ymm.qiankundai.ui.PluginInfoActivity
[Maxim] // 9 - com.wlqq.phantom.library.proxy.ActivityHostProxy$ActivityProxyTranslucent
[Maxim] // 10 - com.ymm.lib.im.linkify.LinkifyRouteActivity
[Maxim] // 11 - com.ymm.xray.debug.RNBizDebugActivity
[Maxim] // 12 - com.ymm.lib.push.getui.PushDispatchActivity
[Maxim] // 13 - com.ymm.qiankundai.ui.ConfigChangeActivity
[Maxim] // 14 - com.ymm.qiankundai.ui.RouterActivity
[Maxim] // Activity of Coverage: 5.033557
[Maxim] // How many Events Dropped: keys=0, pointers=94, trackballs=0, flips=13, rotations=0
[Maxim] // App appears 1 crash, 0 anr, monkey using seed 1603397467406!!!
[Maxim] // Monkey finished
```

### Android Cralwer

https://developer.android.com/training/testing/crawler


### AppCrawler

一个基于自动遍历的app爬虫工具. 支持android和iOS, 支持真机和模拟器. 最大的特点是灵活性. 可通过配置来设定遍历的规则。

项目地址：https://github.com/seveniruby/AppCrawler
百度云盘下载地址：https://pan.baidu.com/s/1dE0JDCH


```
$java -jar appcrawler-2.4.0-jar-with-dependencies.jar # 使用该命令会生成帮助文档
Usage: appcrawler [options]

  -a, --app <value>        Android或者iOS的文件地址, 可以是网络地址, 赋值给appium的app选项
  -e, --encoding <value>   set encoding, such as UTF-8 GBK
  -c, --conf <value>       配置文件地址
  -p, --platform <value>   平台类型android或者ios, 默认会根据app后缀名自动判断
  -t, --maxTime <value>    最大运行时间. 单位为秒. 超过此值会退出. 默认最长运行3个小时
  -u, --appium <value>     appium的url地址
  -o, --output <value>     遍历结果的保存目录. 里面会存放遍历生成的截图, 思维导图和日志
  --capability k1=v1,k2=v2...
                           appium capability选项, 这个参数会覆盖-c指定的配置模板参数, 用于在模板配置之上的参数微调
  -r, --report <value>     输出html和xml报告
  --template <value>       输出代码模板
  --master <value>         master的diff.yml文件地址
  --candidate <value>      candidate环境的diff.yml文件
  --diff                   执行diff对比
  -vv, --verbose           是否展示更多debug信息
  --demo                   生成demo配置文件学习使用方法
  --help                   
示例
appcrawler -a xueqiu.apk
appcrawler -a xueqiu.apk --capability noReset=true
appcrawler -c conf/xueqiu.json -p android -o result/
appcrawler -c xueqiu.json --capability udid=[你的udid] -a Snowball.app
appcrawler -c xueqiu.json -a Snowball.app -u 4730
appcrawler -c xueqiu.json -a Snowball.app -u http://127.0.0.1:4730/wd/hub

#生成demo例子
appcrawler --demo

#启动已经安装过的app
appcrawler --capability "appPackage=com.wlqq,appActivity=.activity.HomeActivity"

#从已经结束的结果中重新生成报告
appcrawler --report result/

#新老版本对比
appcrawler --candidate result/ --master pre/ --report ./

```

示例：
```
java -jar ../appcrawler/appcrawler-2.4.0-jar-with-dependencies.jar --capability "appPackage=com.wlqq,appActivity=.activity.HomeActivity"
```
生成demo配置文件

```
java -jar ../appcrawler/appcrawler-2.4.0-jar-with-dependencies.jar --demo
```


####  执行方式

- 参数模式
  - java -jar <appcrawler.jar路径> --demo  
  - appcrawler --capability "appPackage=com.xueqiu.android,appActivity=.view.WelcomeActivityAlias"
- 配置文件[推荐模式]
  - java -jar <appcrawler.jar路径> -c example.yml --capability"appPackage=com.xueqiu.android,appActivity=.view.WelcomeActivityAlias" \
-o /tmp/xueqiu/1

案例：
```
java -jar ../appcrawler/appcrawler-2.4.0-jar-with-dependencies.jar -c demo.yml
```


执行参数与配置文件

- capability设置:与appium完全一致
- testcase:用于启动app后的基础测试用例
- selectedList:遍历范围设定
- triggerActions:特定条件触发执行动作的设置
- 执行参数比配置文件优先级别高


xpath测试：

- 获取page source
```
adb shell "uiautomator dump && cat /sdcard/window_dump.xml" | sed 1d
```

####  技术原理剖析

技术点：

- 跨平台 -> 基于Appium
- 简化 -> 基于Yaml配置
- 自动化策略 -> 自动遍历+基础的自动化用例支持

与传统WebDriver的不同点

- WebDriver
  - 根据id class xpath进行定位
  - 直接截图
- AppCrawler:ReactWebDriver模式
  - 先getPageSource获取所有的元素列表
  - 根据配置好的宽泛的xpath直接选择元素
  - 然后再生成匹配到的每个元素的唯一定位xpath表达式或者id定位表达式
  - 然后调用appium定位并执行action
  - 截图时增加对选择控件的高亮区分


## 三、自动化测试平台

常见的测试工具与平台：
  
- Android测试设备管理：STF
- Android与iOS任务调度：Selenium Grid
- 持续集成平台：Jenkins
- 测试报告框架：Allure2


整体架构：
![](_v_images/20201020154954995_1506306529.png)


实施技术架构：
![](_v_images/20201020155103578_1956602067.png)



智能设备选型：
![](_v_images/20201020155157603_193853558.png)



### STF平台

- https://github.com/openstf/stf
- 面向自动化测试：远程调试功能是常用的功能
- 手工测试：交互调试

安装：
```
$brew install rethinkdb graphicsmagick zeromq protobuf yasm pkg-config #安装依赖
$npm install -g stf
```

nvm安装教程：https://www.jianshu.com/p/04141aeaf218

stf 安装路径：/usr/local/Cellar/nvm/0.34.0/versions/node/v8.12.0/lib/node_modules/stf/bin
启动stf：
```
$rethinkdb  #首先启动数rethinkdb服务
$stf local
登录地址：http://localhost:7100
```

启动成功后的界面如下：
![](_v_images/20201020175146042_364056807.png =1278x)


输入一个用户名和邮箱，即可登录。
![](_v_images/20201020181407612_521844946.png)




STF APIs

- https://github.com/openstf/stf/blob/master/doc/API.md


生成token：
```
4f1df320e9954893b0896d6a0da6ed30ec846cbb4b684d3cbc74f270e1dab008
```

获取用户信息：
```
curl -H "Authorization: Bearer 4f1df320e9954893b0896d6a0da6ed30ec846cbb4b684d3cbc74f270e1dab008" http://127.0.0.1:7100/api/v1/user |jq

返回结果：
{
  "success": true,
  "user": {
    "createdAt": "2020-10-20T09:52:06.108Z",
    "email": "larry616@sina.cn",
    "forwards": [],
    "group": "SWK+bw0qREKvsNEgbhtWPA==",
    "ip": "::ffff:127.0.0.1",
    "lastLoggedInAt": "2020-10-20T09:58:46.194Z",
    "name": "larry",
    "settings": {
      "deviceListActiveTabs": {
        "details": true,
        "icons": false
      },
      "lastUsedDevice": "28d6f388",
      "selectedLanguage": "zh_CN"
    }
  }
}

```
对设备操作进行封装成脚本
```
#!/usr/bin/env bash

# set -euxo pipefail

DEVICE_SERIAL=emulator-5554
STF_URL=http://localhost:7100
STF_TOKEN=34982fcd4c0c4f6cb2f5bb82222655a98ec3c690e7854d4ebcd00eb88125d795

if [ "$DEVICE_SERIAL" == "" ]; then
    echo "Please specify device serial using ENV['DEVICE_SERIAL']"
    return 1
fi

if [ "$STF_URL" == "" ]; then
    echo "Please specify stf url using ENV['STF_URL']"
    return 1
fi

if [ "$STF_TOKEN" == "" ]; then
    echo "Please specify stf token using using ENV['STF_TOKEN']"
    return 1
fi

function add_device() {
    response=$(curl -X POST -H "Content-Type: application/json" \
        -H "Authorization: Bearer $STF_TOKEN" \
        --data "{\"serial\": \"$DEVICE_SERIAL\"}" $STF_URL/api/v1/user/devices)

    success=$(echo "$response" | jq .success | tr -d '"')
    description=$(echo "$response" | jq .description | tr -d '"')

    if [ "$success" != "true" ]; then
        echo "Failed because $description"
        return 1
    fi

    echo "Device $DEVICE_SERIAL added successfully"
}

function remote_connect() {
    response=$(curl -X POST \
        -H "Authorization: Bearer $STF_TOKEN" \
        $STF_URL/api/v1/user/devices/$DEVICE_SERIAL/remoteConnect)

    success=$(echo "$response" | jq .success | tr -d '"')
    description=$(echo "$response" | jq .description | tr -d '"')

    if [ "$success" != "true" ]; then
        echo "Failed because $description"
        return 1
    fi
    remote_connect_url=$(echo "$response" | jq .remoteConnectUrl | tr -d '"')

    adb connect $remote_connect_url

    echo "Device $DEVICE_SERIAL remote connected successfully"
}
function remote_disconnect() {
    local remote_device=$(curl -H "Authorization: Bearer $STF_TOKEN" \
        $STF_URL/api/v1/user/devices |
        jq -c '.devices[]|[.remoteConnectUrl,.serial]' |
        awk -F\" -v serial=$DEVICE_SERIAL '$4==serial{print $2}')
    [ -n "$remote_device" ] && adb disconnect $remote_device
}

function remove_device() {
    response=$(curl -X DELETE \
        -H "Authorization: Bearer $STF_TOKEN" \
        $STF_URL/api/v1/user/devices/$DEVICE_SERIAL)

    success=$(echo "$response" | jq .success | tr -d '"')
    description=$(echo "$response" | jq .description | tr -d '"')

    if [ "$success" != "true" ]; then
        echo "Failed because $description"
        return 1
    fi

    echo "Device $DEVICE_SERIAL removed successfully"
}

function udid() {
    DEVICE_SERIAL=$1
}

for p in "$@"; do
    $p
done

```



测试流程

- 申请设备
- 申请远程连接
- udid=xxx pytest xxxx
- 释放远程连接
- 释放设备


脚本演示：
```
$. stf.sh
$udid=28d6f388
$add_device #申请设备
$remote_connect # 申请远程连接
$remote_disconnect #释放远程连接
$remove_device  #释放设备
```

### selenium grid方案


- 支持Android、iOS模拟器
- 支持web浏览器
- 支持所有兼容webdriver协议的框架
- 不支持在线调试


使用selenium grid做自动化

- 与普通的自动化用例无区别
- URL修改为对应的hub的URL即可

grid下载地址

- https://www.selenium.dev/downloads/

![](_v_images/20201021100129287_2041247691.png)


首先启动hub进行管理：
```
$java -jar selenium-server-standalone-3.141.59.jar -role hub
```
启动成功，可以本地浏览器进行访问：http://10.250.116.151:4444/grid/
![](_v_images/20201021100333695_1198681812.png)


然后注册节点，首先示例web版注册
```
$java -jar selenium-server-standalone-3.141.59.jar -role node -hub http://localhost:4444
```

注册成功后，hub可以看到信息
![](_v_images/20201021100528017_701121777.png)


注册appium节点，如下

配置参考文档：http://appium.io/docs/en/advanced-concepts/grid/#selenium-grid

注册node的配置格式：
```
{
  "capabilities":
      [
        {
          "browserName": "<e.g._iPhone5_or_iPad4>",
          "version":"<version_of_iOS_e.g._7.1>",
          "maxInstances": 1,
          "platform":"<platform_e.g._MAC_or_ANDROID>"
        }
      ],
  "configuration":
  {
    "cleanUpCycle":2000,
    "timeout":30000,
    "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
    "url":"http://<host_name_appium_server_or_ip-address_appium_server>:<appium_port>/wd/hub",
    "host": "<host_name_appium_server_or_ip-address_appium_server>",
    "port": <appium_port>,
    "maxSession": 1,
    "register": true,
    "registerCycle": 5000,
    "hubPort": <grid_port>,
    "hubHost": "<Grid_host_name_or_grid_ip-address>"
    "hubProtocol": "<Protocol_of_Grid_defaults_to_http>"
  }
}
```

注册node命令：
```
$appium -p 5723 --nodeconfig appium.json 
```
![](_v_images/20201021102449103_510934950.png)


用途：

- Central entry point for all tests
- Management and control of the nodes / environment where the browsers run
- Scaling
- Running tests in parallel
- Cross platform testing
- Load balancing


### grid与stf的对比
![](_v_images/20201021105120837_1644083944.png =967x)


**node的匹配规则**
grid如何找node， 首先node先找hub注册。注册的时候表示了自己的capability。
client运行脚本的时候，也会发送一个脚本里的capability。
node的capability与client的capability必须对应。如果client里声明了一个capability在node中没有，那么就不会选择任何node


**client 设备的选择：**

- 如果没有udid，那么默认选择adb devices列表的第一个
- 如果有，那么就执行特定的udid，找不到就报错。udid可以通过环境变量传递到脚本中
```
udid=os.getenv("udid", None)
if udid is not None:
   caps["udid"] = os.getenv("udid", "")
```


**测试流程**

- 为每台设备注册一个节点
- 节点注册到hub
- udid=xxx pytest xxxx