# Hello World

## 从HelloWorld开始

  我们写程序的时候都是从HelloWorld开始的,HelloWorld往往代表了最简单的程序是如何构建和编写的,<br>
那么我们这里也从HelloWorld开始.

## 准备工作

  * 一台Linux(WinServer不会玩)云主机,最好功能完整.(PS:很多免费的云主机完全不能用,最好不要去申请这个,推荐阿里云,基本配置足够)
  * 一台手机,苹果三星(iOS/Android)都可以.(PS:WP暂时不支持,不要问我为什么)
  * 一个树莓派+一个Arduino(PS:树莓派我用的是2代B型，其他应该也可以，但是在引脚上略有不同)
  * Linux相关知识,希望你最起码会基本使用

##　配置云主机
  首先你要先安装Python,阿里云主机自带Python2.7和Python3.4.3,你可以执行下面命令来验证:
>abcd@Linux:~/IOT_Practice$ python<br>
    Python 2.7.6 (default, Jun 22 2015, 17:58:13)<br>
    [GCC 4.8.2] on linux2<br>
    Type "help", "copyright", "credits" or "license" for more information.<br>
    \>>>

>abcd@Linux:~/IOT_Practice$ python3<br>
Python 3.4.3 (default, Oct 14 2015, 20:28:29)<br>
[GCC 4.8.4] on linux<br>
Type "help", "copyright", "credits" or "license" for more information.<br>
\>>>

  基本环境已经可以了,下面我们需要安装[pip](http://pypi.python.org/),一条命令即可:
>abcd@Linux:~/IOT_Practice$ sudo apt-get install pip

  pip用于依赖包的管理.Python官方建议我们不要在生产环境中直接安装各个依赖包和执行自己的程序,所以就有了<br>
虚拟环境[virtualenv](https://virtualenv.readthedocs.org/en/latest/),他可以做到:
  * 在没有权限的情况下安装新套件
  * 不同应用可以使用不同的套件版本
  * 套件升级不影响其他应用
  安装步骤也是一条命令:
> abcd@Linux:~/IOT_Practice$ sudo pip install virtualenv

  与以往不同的是,我们这里用到Python3的环境,所以需要指定Python的版本,于是用到下面的命令:
> abcd@Linux:~/IOT_Practice$ virtualenv -p /usr/bin/python3 venv3
Running virtualenv with interpreter /usr/bin/python3
Using base prefix '/usr'
New python executable in venv3/bin/python3
Also creating executable in venv3/bin/python
Installing setuptools, pip, wheel...done.

  这样我们就安装好了.下面我们需要做的就是激活这个虚拟环境,用到下面的命令:
> abcd@Linux:~/IOT_Practice$ source ./venv3/bin/activate
  (venv3)abcd@Linux:~/IOT_Practice$

  这样你就看到我们已经位于虚拟环境下了,我们终于可以开始工作了!我们下面进入服务器的安装,其实我们并没有对它进行定制,<br>
只是先从最简单的开始,这里我们使用的是[hbmqtt](https://github.com/beerfactory/hbmqtt),一条命令即可:
> abcd@Linux:~/IOT_Practice$ pip install hbmqtt
Collecting hbmqtt
  Using cached hbmqtt-0.6-py34-none-any.whl
Collecting docopt (from hbmqtt)
Collecting pyyaml (from hbmqtt)
Collecting websockets (from hbmqtt)
  Using cached websockets-3.0-py33.py34.py35-none-any.whl
Collecting passlib (from hbmqtt)
  Using cached passlib-1.6.5-py2.py3-none-any.whl
Collecting transitions==0.2.5 (from hbmqtt)
Installing collected packages: docopt, pyyaml, websockets, passlib, transitions, hbmqtt
Successfully installed docopt-0.6.2 hbmqtt-0.6 passlib-1.6.5 pyyaml-3.11 transitions-0.2.5 websockets-3.0

  这样就安装好了,启动的话也很简单,直接执行hbmqtt即可,如下:
> abcd@Linux:~/IOT_Practice$ hbmqtt

  到此,服务器端配置结束!
