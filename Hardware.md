# 硬件电路搭建
  这一章主要说明下硬件电路搭建的问题,在开始之前我先讲一下我们搭建的方案.我暂时确定的室内控制器件是:<br>
  * 温度湿度传感器
  * 人体红外传感器
  室外的是:<br>
  * 光敏电阻传感器
  * 摄像头(主要放在门口,监控以及人脸识别,暂时先不用)
  温度湿度主要是检测室内环境,人体红外可以用在浴室中人进入时开启浴霸的操作;光敏电阻放在室外的话检测时间是否是晚上还是白天,放在室内的话做一个光线检测的装置;<br>
摄像头目前还是想着用在监控上,想添加人脸识别的内容.我们这一次看上去虽然比**HelloWorld**章节中的内容要复杂,但究其原理的话还是一样,一个是监控,一个是控制,<br>
控制的话我们在Arduino的基础上接入继电器,利用继电器的作为开关控制空调,台灯,电视等电器的电源开闭.这样我们就暂时确定这几种传感器类型,后续的应用程序也基于它们来进行开发!

##
