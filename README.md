# Weather_station-Nbiot-Huaweicloud
# GIE个人气象站
![Photo](https://github.com/linzi0928/Weather_station-Nbiot-Huaweicloud/blob/master/%E6%88%90%E5%93%81%E7%85%A7%E7%89%87.jpg)  
硬件结构：  
STM32L051低功耗单片机  
BME680气体传感器测量温度、湿度、气压、烟阻  
ZPH02粉尘传感器测量PM2.5  
VEML6070紫外线传感器测量紫外线  
BC35-G模块通过NB-Iot接入华为云OceanConnect  
低功耗运作方式：wakeup计时器每15s唤醒一次喂看门狗（28s未喂狗则重启单片机），然后立即进入STOP模式，当过去225个15s唤醒周期后发送一次数据（实测差不多为1小时一次数据）  
待机状态整机电流：~4uA  
工作状态整机电流：~80mA（每60分钟工作上报一次数据）  
此种配置方式搭配串联的两块2.8V 3000F超级电容，3W9V的小太阳能板在持续阴天条件下电压也不会掉，可考虑在不改变硬件条件下缩短上报间隔。  
稳压方案：单片机使用XC6206P182超低压差1.8V LDO稳压供电，传感器、无线模块使用自动升降压芯片TPS63070，PM2.5传感器需要5V供电，使用MT3608升压驱动  

显示终端方案：基于Android Studio开发的安卓APP，记录所有接收到的数据，并将最近24小时的温度、紫外线数据以折线图形式显示，最近一次获取的温度、湿度以仪表盘形式显示，其余信息以普通字符方式显示，首次使用时需要填入在华为OceanConnect上注册应用时所给的应用ID和应用Secret以及节点ID，配置文件将保存到安卓主目录下    
  
注意：  
如果在CUBEMX中重新生成代码，注释将全部变成乱码，请提前备份，传感器相关程序在sensors.c文件中  
Android Studio的工程文件夹尽量放在磁盘根目录，不可太深否则编译器找不到  
Android Studio编译前执行一遍Clean Project可能会解决部分配置错误  
建议不要手动修改app生成的数据和记录文件，否则可能导致闪退  
SIM卡必须使用NB-IOT专用卡，我使用的是移动的NB卡，普通物联网卡、上网卡不可使用！  
欢迎关注b站up主：GIE工作室，获得超多干货！  
GIE工作室 2020年2月9日第一版 版权所有 严禁用于任何商业用途  

附引脚：  
TLV61220（5V输出使能脚） PC13  
TPS63070(3.3V使能脚）PA0  
SDA680（SPI2_MOSI） PB15  
SDO680（SPI2_MISO） PB14  
SCL680  PB13  
CS680   PB12  
SCL6070 PB3  
SDA6070 PB4  
  
更新说明：  
V2.6 20200311  
修复程序中PM2.5模块获取失败超过20次将导致锁死问题，APP新增绘制历史查询曲线功能，曲线图不再是读取本地已有数据而是读取服务器过去24小时真实数据，新增刷新按钮  
  
V2.5 20200309  
修复了PCB右边上拉电阻接错导致模块与单片机无法通信的问题，将烟阻最后一位用于传输VOC等级（需ZPH02模块支持）  
  
V2.4 20200305  
APP新增历史数据查询功能，修正PM2.5单位问题，优化了PM2.5模块测量策略  
  
V2.3 20200303  
由于5V升压TLV61220驱动能力不足，导致PM2.5模块一直处于异常的工作状态，将TLV61220更换为MT3608，去除多余的mos管AO3400  
  
V2.2 20200301  
APP添加手机竖屏版本  
  
V2.1 20200225  
修正调试口的电源问题，将调试口电源更换为1.8V  
  
V2.0 20200210（b站开源版本）  
布局大改，去除四角无用的螺丝孔，单片机改为1.8V LDO直供，TPS63070无需一直处于开启状态，修正天线接口过高导致无法装盒问题，优化PCB大小，去除电平转换芯片TXS0102，去除串口上串接的2K电阻，串口通信使用开漏+上拉电阻形式  

V1.0 20191224（初始版本）  
