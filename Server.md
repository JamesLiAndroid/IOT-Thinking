# 服务器端定制开发
  由于我们采用的[hbmqtt](https://github.com/beerfactory/hbmqtt)这个开源项目中已经实现了MQTT协议的基本功能,而且可以定制开发的东西比较少,我们需要做的是:<br>
  * 日志信息的监控记录,需要在双端(服务端,RaspberryPi端)进行记录
  * 配置文件,包含配置外网访问,ssl安全等等,我们先做一个简单的配置

## 日志记录
  由于我们启动服务器端代码是在控制台启动的,如果我们将这个代码作为Service在服务器启动时就开始运行代码,那么日志信息不会显示在控制台,这时我们需要记录在文件中,<br>
以便我们监控服务器运行.

  那么我们先讲一下输出到控制台的配置,代码如下:

    import logging

    console = logging.StreamHandler()
    console.setLevel(logging.DEBUG)
    formatter = logging.Formatter('[%(asctime)s] :: %(levelname)s :: %(name)s :: %(message)s')
    console.setFormatter(formatter)
    logging.getLogger('').addHandler(console)

  在使用的过程中,在需要输出日志的位置,直接调用

    logging.debug('填写你需要的信息')
    logging.info('填写你需要的信息')
    logging.warning('填写你需要的信息')
    logging.error('填写你需要的信息')

  这样日志信息就会输出到控制台了．
  输出到文件和输出到控制台的配置比较相似,代码如下:

    import logging

    console = logging.FileHandler('application.log') # 就是StreamHandler换成FileHandler,然后指定文件名称
    console.setLevel(logging.DEBUG)
    formatter = logging.Formatter('[%(asctime)s] :: %(levelname)s :: %(name)s :: %(message)s')
    console.setFormatter(formatter)
    logging.getLogger('').addHandler(console)

  application.log会保存在当前文件夹下,这样调用的话,和前面一致,但是会把日志信息输出到文件中!

##　配置文件
  hbmqtt使用[yaml](http://pyyaml.org/wiki/PyYAMLDocumentation)语言来编写配置文件,下面我们先编写一个简单的yaml文件:
  > config.yaml
    listener:
        default:
            max-connections: 5000 # 最大连接数
            type: tcp  # tcp方式连接
            bind: 0.0.0.0:1883 # 访问ip以及端口的指定

  我们先简单的配置下默认方式,这样编写完成之后,我们在服务器端调用的时候是这样的,代码如下:

    import yaml
    stream = open('./config.yaml','r') # 以只读方式打开文件
    yaml_conf = yaml.load(stream) # 载入文件,返回值是dictionary类型

    from hbmqtt.broker import broker
    broker = Broker(yaml_conf) # 初始化时传入

  这样我们的服务器就配置好了,更复杂的配置在[hbmqtt](http://hbmqtt.readthedocs.org/en/latest/references/broker.html)文档中有介绍!

# 最终代码
  首先,需要在你的服务器上当前目录下编写一个yaml文件,例如上面提到的config.yaml,然后下面附上完整的服务端代码:

    import logging
    import asyncio
    import os
    from hbmqtt.broker import Broker

    def init_log():
        console = logging.FileHandler('application.log') # 就是StreamHandler换成FileHandler,然后指定文件名称
        console.setLevel(logging.DEBUG)
        formatter = logging.Formatter('[%(asctime)s] :: %(levelname)s :: %(name)s :: %(message)s')
        console.setFormatter(formatter)
        logging.getLogger('').addHandler(console)

    stream = open('/home/ubuntu/IotProject/server/config.yaml','r')
    yaml_conf = yaml.load(stream)
    broker = Broker(yaml_conf)

    @asyncio.coroutine
    def test_coro():
        yield from broker.start()

    if __name__ == '__main__':
        formatter = "[%(asctime)s] :: %(levelname)s :: %(name)s :: %(message)s" # 基本配置,输出到控制台
        logging.basicConfig(level=logging.INFO, format=formatter)
        init_log() # 输出到文件
        asyncio.get_event_loop().run_until_complete(broker_coro())
        asyncio.get_event_loop().run_forever()

    这样我们完整的代码就写完了!

# 配置服务器开机启动
  如果需要配置服务器开机启动的话,我们这里用一个脚本来执行,脚本代码如下:

    #!/bin/bash
    source /home/ubuntu/IotProject/venv/bin/activate
    python /home/ubuntu/IotProject/server/server.py

  在这里需要强调的是,第一个source执行的命令表示我们需要先开启python的虚拟环境,第二条命令是执行python脚本.,这里必须是绝对路径.<br>
因为后续我们需要配置为开机启动项,让服务器代码常驻后台运行!首先我们需要把启动脚本复制或软连接到/etc/init.d/目录下,我这里用的复制的方式:

    sudo cp start.sh /etc/init.d/

  现在我们需要配置开机启动,我们需要切换到/etc目录下,操作该目录下的rc.local文件中的内容,通过vim打开,下面演示这些操作:

    cd /etc
    sudo vim rc.local

  这样我们就可以对rc.local文件进行编辑,添加我们的脚本启动代码,代码很简单,只有一句话:

    sudo sh /etc/init.d/start.sh

  注意,这一句一定放在**exit 0**之前,一定要添加在它前面,否则这句话不会被执行,然后服务器中也就看不到我们的脚本执行!<br>
  这样我们基本的配置就做完了,这里我们从控制台重启阿里云服务器!重启完毕后,ssh登陆服务器,运行下面命令:

    ps -ef

  列举相关进程,这时我们会看到这样的结果出现:

    root       857     1  0 09:29 ?        00:00:00 python /home/ubuntu/Iot-Project/server/serins.py
  证明我们成功开启了,服务端代码,这时候他就常驻后台运行了!
