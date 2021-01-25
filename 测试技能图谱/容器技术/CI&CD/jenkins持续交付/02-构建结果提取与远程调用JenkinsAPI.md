# 02-构建结果提取与远程调用JenkinsAPI


## 构建结果提取


通过参数提取Jenkins任务构建结果- $BUILD_STATUS

![](_v_images/20210119111535464_907187613.png)



如何提取内容细节？ 如下：

![](_v_images/20210119111656692_2115801638.png)



提取方式：

- 使用Editable Email Notification 插件
- 利用参数 $BUILD_LOG_REGEX 中的正则表达式功能，从LOG中匹配测试结果
- 正则表达式参考Tests：run ([0-9]*),Failures([0-9]*),Errors([0-9]*),Skipped([0-9]*)


案例：jenkins构建结果提取：参数提取+正则提取

1、首先找到Editable Email Notifacation
![](_v_images/20210119144931456_96419193.png)

2、进入高级设置

![](_v_images/20210119145015944_1314706913.png)

3、在Triggers选择高级

![](_v_images/20210119145051688_819842808.png)


4、 在Subject栏目输入如下内容：

```
$BUILD_STATUS: ${BUILD_LOG_REGEX, regex="Tests run: ([0-9]*), Failures: ([0-9]*), Errors: ([0-9]*), Skipped: ([0-9]*),.*" ,showTruncatedLines=false}
```

可以使用在线网址测试正则表达式有无问题：https://tool.chinaz.com/regex

5、保存即可。


接下来我们构建任务，查看邮件标题是否输出我们匹配的结果！

![](_v_images/20210119145311390_842994904.png)


可以发现匹配的结果就显示在邮件标题被发送了！



**小结：**


- 传统方法：Jenkins参数提取构建结果， --成功 or 失败
- 灵活方法： 正则表达式来匹配自己需要的结果内容
- 传统方法容易上手，灵活方法展示内容更丰富，根据自己需要来选择



## jenkins API


**jenkins  API简介**

- jenkins对外可以暴露动作交互入口
- 为外部程序提供入口，可以控制jenkins
- 支持协议-http
- API接口支持用户名、密码认证
- Jenkins API支持的典型功能：运行job、查看任务状态、返回任务编号......等等


**调用jenkins API环境准备**


- 创建一个有任务运行和任务状态查询等用户
- 准备一个打算通过API远程控制的任务



案例一：远程调用jenkins API任务

- 任务名：TestEmail
- 方法：POST
- 封装：JenkinsAPI
- 过程： 获取任务实例，运行任务
- demo，代码放在最后

我们先创建一个TestMail任务！

![](_v_images/20210119172908947_1265732269.png)


在加上email邮件配置，保存即可。我们运行测试下！

![](_v_images/20210119173242373_189237901.png)



实例2 ：远程调用Jenkins API返回最新任务编号

任务名：TestMail

远程API服务地址：
```
http://47.108.156.7:6001/job/TestMail/lastBuild/buildNumber
```
请求方法：GET



用户名、密码添加方法：username:password@hostname:port.....

运行期望结果：
1、最新任务build number
2、服务返回http status：200


浏览器输入拿到结果如下：
![](_v_images/20210119174105116_98120900.png)


这里需要一个注意点，我们直接接口调用是需要加用户名和密码的。如果不加出现结果如下：
![](_v_images/20210119174451209_1998009051.png)


我们在URL加上密码再进行测试

![](_v_images/20210119174549362_336332018.png)


可以发现获取到最后任务的编号了！


实例3: 远程调用jenkins API查询任务状态

- 任务名：TestMail
- 远程API服务地址

```
http://test:test@47.108.156.7:6001/job/TestMail/2/api/json
```  

- 请求方法：GET
- 用户名、密码添加方法：username:password@host:port....
- 任务运行期望结果：
  - 任务详情JSON
  - 服务返回http status 200
 
 接口查询如下：
 ![](_v_images/20210119175318351_555267622.png)



综合案例：利用代码远程控制jenkins任务

任务名：TestEmai
实现目标：启动任务、检查任务状态、输出任务结果
语言：python


实现代码如下：

```
# 需要安装python-jenkins 三方依赖库
from pprint import pprint
import logging
import jenkins
import time


class iJenkins:
    def __init__(self, url, username, password):
        self.url = url
        self.usermane = username
        self.password = password
        logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s : %(message)s',
                            datefmt='%a, %d %b %Y %H:%M:%S')
        self.logger = logging.getLogger()

    # 创建链接获取server对象
    def conncet_jenkins(self):
        server = jenkins.Jenkins(self.url, username=self.usermane, password=self.password)
        return server

    # 根据job名称，获取最后一个编号
    def get_job_last_number(self, job_name):
        server = self.conncet_jenkins()
        job_info = server.get_job_info(job_name)
        return job_info['lastBuild']['number']

    # 获取job构建结果详情
    def get_job_build_info(self, job_name):
        number = self.get_job_last_number(job_name)
        server = self.conncet_jenkins()
        job_build_info = server.get_build_info(job_name, number)
        build_result = {
            'building': job_build_info['building']  # 获取任务构建是否正在构建，构建中结果为True
            , 'result': job_build_info['result']  # 获取任务结果，构建中为None
        }
        return build_result

    # 构建任务
    def run(self, job_name):
        server = self.conncet_jenkins()
        server.build_job(job_name)

    # 构建任务完整步骤
    def run_build(self, job_name):
        # 构建前获取任务最后编号
        old_number = self.get_job_last_number(job_name)
        # 执行构建
        self.run(job_name)
        self.logger.info(f'Start running the job {job_name}')
        # 获取当前执行任务的编号
        current_number = self.get_job_last_number(job_name)
        # 检查构建任务是否完成
        start = time.time()
        while not current_number > old_number:
            # wait for the latest number update
            time.sleep(1)
            current_number = self.get_job_last_number(job_name)
            if time.time() - start >= 25:
                raise TimeoutError(f'Get new build number timeout Error, timeout > 25s')
        self.logger.info(f'The new build instance number is {current_number}')
        start = time.time()
        # 判断任务构建状态，如果是构建中，则一直循环输出信息，并且判断时间
        while self.get_job_build_info(job_name).get('building'):
            self.logger.info(f'The {job_name}\'s building is on-going .....')
            time.sleep(3)
            if time.time() - start >= 25:
                raise TimeoutError('Run build timeout Error, timeout > 25s')
        result = self.get_job_build_info(job_name).get('result')
        self.logger.info(f'The {job_name}\'s #{current_number} building result is {result}')


if __name__ == '__main__':
    url = 'http://47.108.156.7:6001'
    username = 'test'
    pwd = 'test'
    job_name = 'TestMail'
    server = iJenkins(url, username, pwd)
    pprint(server.conncet_jenkins().get_job_info('TestMail'))
    # print(server.get_job_last_number(job_name))
    # pprint(server.get_job_build_info(job_name))
    server.run_build(job_name)
```

运行结果如下：

```
/Users/user/PycharmProjects/SDET11-LY/venv/bin/python3 /Users/user/PycharmProjects/SDET11-LY/jenkinstest/iJen.py
Wed, 20 Jan 2021 10:00:45 - INFO : Start running the job TestMail
Wed, 20 Jan 2021 10:00:53 - INFO : The new build instance number is 8
Wed, 20 Jan 2021 10:00:54 - INFO : The TestMail's building is on-going .....
Wed, 20 Jan 2021 10:00:56 - INFO : The TestMail's building is on-going .....
Wed, 20 Jan 2021 10:00:58 - INFO : The TestMail's building is on-going .....
Wed, 20 Jan 2021 10:01:01 - INFO : The TestMail's building is on-going .....
Wed, 20 Jan 2021 10:01:04 - INFO : The TestMail's #8 building result is SUCCESS
```

