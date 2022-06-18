---
title: OpenAIE
date: 2022-06-18 13:13:13
author: caffx
tags:
    - Python
categories:
    - 套件
---
# 板载按键
*板载两个用户按键 button1 和 button2，按键按下为低电平，弹起为高电平。*
```python
from openaie import button1, button2
button1.is_press()
button2.is_press()
```
按键按下返回 True，按键弹起返回 False。

# 板载无源蜂鸣器
可通过不同频率的信号驱动，发出不同音调的声音。
```python
from openaie import buzzer
buzzer.tone(freq)
buzzer.no_tone()
```
freq: 频率。
<!-- more -->

# 板载 2.4-inch，240*320 显示屏。
```python
import lcd
lcd.init()
# 初始化
lcd.clear(color)
# 清空显示
lcd.rotation(r)
# 设置显示方向
lcd.set_backlight(val)
# 设置背光亮度
lcd.draw_string(x, y, string, fc=(r,g,b), bc=(r,g,b))
# 画字符
lcd.draw_line(x0, y0, x1, y1, color=(r,g,b), thickness=1)
# 画线
lcd.draw_rectangle(x, y, w, h, color=(r,g,b), thickness=1, fill=0)
# 画矩形
lcd.draw_circle(x, y, radius, color=(r,g,b), thickness=1, fill=0)
# 画圆
lcd.display()
# 显示
```
r: 0-4。0、2为竖屏显示，320\*240。1、3为横屏显示，240\*320。
fc: 字体颜色，
bc: 背景颜色，
thickness: 线宽，
fill: 是否填充。

# LED灯
```python
from openaie import rgb_led
rgb_led(port, num=3)
rgb_led.set(id, (r, g, b))
rgb_led.display()
```
num: 灯珠数量，
id：板载灯编号，
**rgb_led 是个类，需要初始化。**

# 按键组
*按键按下为低电平，弹起为高电平。*
```python
from openaie import button_group
class button_group(port)
button_group.is_press(index)
```

# 电位器
该模块为可调电阻，通过旋转旋钮可改变输出端与两端的阻值，从而改变输出端与两端之间的电压。 通过ADC（模数转换）外设可检测电压变化。
```python
from openair import potentiometer
class potentiometer(port)
potentiometer.read()
```

# 超声波测距传感器
*量程：2-200cm，误差：±1%。*
```python
from openaie import ultrasonic
class ultrasonic(port)
ultrasonic.measure()
ultrasonic.read()
```
回波信号不大于 100ms。

# 温湿度传感器
*温度测量范围：-40-85℃，误差：±0.3%。湿度测量范围：0-100%，误差：±2%。*
```python
from openaie import th_sensor
class th_sensor(port)
th_sensor.read_temperature()
th_sensor.read_humidity()
```

# 可调速电机风扇
```python
from openaie import motor_fan
class motor_fan(port)
motor_fan.set(val)
```
val: 0-100。

# 语音识别及交互模块
```python
from openaie import asr
class asr(port)
asr.read()
```

# AI 模块
```python
from openaie import enlighten_com
class enlighten_com(port)
enlighten_com.wait_connect()
# 等待连接
enlighten_com.set_mode(m)
# 设置模式
enlighten_con.request_data(i)
# 请求数据
enlighten_com.read_qrcode_info()
# 读取二维码扫描结果
enlighten_com.read_blob_info()
# 读取色块信息
enlighten_com.read_detect_info()
# 读取检测结果
```
**m:**

* "qrcode scan": 二维码扫描
* "mask detect": 口罩检测
* "image class"：图像检测分类
* "color recognition": 颜色识别

**i:**

* "qrcode": 二维码信息
* "mask detect": 口罩检测结果
* "image class": 图像检测分类结果
* "red blob", "green blob", "blue blob": 红/绿/蓝色块

read_blob_info: [x, y, w, h, pixels]
read_detect_info: [x, y, w, h, classid, confidence]

口罩检查模型的分类标签映射(label_map)
{0: 'unmask', 1: 'mask'}
图像检测分类模型的分类标签映射(label_map)
{0:'airplane',
1:'bicycle',
2:'bird',
3:'steamship',
4:'bootle',
5:'bus',
6:'car',
7:'cat',
8:'cheer',
9:'ox',
10:'dining-table',
11:'dog',
12:'horse',
13:'motobike',
14:'person',
15:'flowerpot',
16:'sheep',
17:'sofa',
18:'train',
19:'tetevision'}

# GPS/北斗双模定位导航模块
*通信接口UART，默认波特率：9600，数据位8，停止位1。数据格式为 NEMA0183 标准，输出频率为1Hz。板载指示灯在定位成功时闪烁。定位精度10m，首次定位时间大于32秒。*
```python
from openaie import bds
class bds(port)
bds.update()
# 更新数据

# -- Variables --
bds.satellites_in_view # 可见卫星数
bds.satellites_in_use # 使用卫星数
bds.longitude[0] # 经度(float)
bds.latitude[0] # 纬度(float)
bds.altitude # 海拔(m)
bds.speed[2] # 速度(km/h)
day, month, year = bds.date[:] # 日期(UTC+8)；日，月，年
hour, minute, second = bds.timestamp[:] # 时间戳(UTC+8)；时，分，秒
```

# 示例
## 按键检测
```python
import time
from openaie import button1, button2
while True:
    if button1.is_press():  # 检测到按键按下
        time.sleep_ms(10)  # 延时消抖
        if button1.is_press():
            print("button 1 press")
        while (button1.is_press()):  # 等待按键释放
            pass
    if button2.is_press():  # 检测到按键按下
        time.sleep_ms(10)  # 延时消抖
        if button2.is_press():
            print("button 2 press")
        while (button2.is_press()):  # 等待按键释放
            pass
```
## 鸣响测试
```python
import time
from openaie import buzzer

tone_list = (289, 661, 700, 786, 882, 990, 1112)  # Do、Re、Mi、Fa、Sol、La、Si
for i in range(7):
    buzzer.tone(tone_list[i])
    time.sleep_ms(500)
    buzzer.no_tone()
```
## 绘图测试
```python
# 导入显示模块
import lcd
# 设置背光亮度
lcd.set_backlight(80)
# 清空显示
lcd.clear(color=(0, 0, 0))
# 设置显示方向
lcd.rotation(0)
# 画线
lcd.draw_line(10, 10, 80, 80, color=(0, 255, 0), thickness=5)
# 画矩形
lcd.draw_rectangle(100, 20, 80, 60, color=(255, 0, 0), thickness=8, fill=0)
# 画圆形，实心
lcd.draw_circle(120, 160, 30, color=(0, 0, 255), thickness=1, fill=1)
# 显示字符
lcd.draw_string(100, 200, "lcd draw test", fc=(0, 255, 0), bc=(0, 0, 0))
# 输出显示
lcd.display()
```
## 点亮LED
```python
import time
from openaie import rgb_led  # 导入模块

rgb = rgb_led(1)  # 端口1
rgb.set(0, (50, 0, 0))  # 将第1个灯设置为红色亮度值为：50
rgb.set(1, (0, 60, 0))  # 将第2个灯设置为绿色亮度值为：60
rgb.set(2, (0, 0, 70))  # 将第3个灯设置为蓝色亮度值为：70
rgb.display()  # 输出显示
time.sleep_ms(1000)
color = ((50, 0, 0), (0, 50, 0), (0, 0, 50), (0, 0, 0))
for i in range(4):
    rgb.set(0, color[i])
    rgb.set(1, color[i])
    rgb.set(2, color[i])
    rgb.display()
    time.sleep_ms(500)
```
## 按键状态检测
```python
import time
from openaie import button_group  # 导入按键模块

bt2 = button_group(2)  # 按键模块连接到端口2while True:
if bt2.is_press(1):
    print("press")
else:
    print("release")
time.sleep_ms(100)
```
## 按键控制开关灯
```python
import time
from openaie import button_group, rgb_led

rgb = rgb_led(1)
bt2 = button_group(2)
while True:
    if bt2.is_press(1):  # 检测到按键1按下
        time.sleep_ms(10)
        if bt2.is_press(1):
            print("button 1 press")
            rgb.set(0, (0, 0, 50))
            rgb.set(1, (0, 0, 0))
            rgb.display()
        while bt2.is_press(1):  # 等待按键1释放
            pass
    if bt2.is_press(2):  # 检测到按键2按下
        time.sleep_ms(10)
        if bt2.is_press(2):
            print("button 2 press")
            rgb.set(0, (0, 0, 0))
            rgb.set(1, (0, 0, 50))
            rgb.display()
        while bt2.is_press(2):  # 等待按键2释放
            pass
```
## 数值读取
```python
import time
from openaie import potentiometer

p = potentiometer(3)  # 电位器连接 端口3
while True:
    print("val: ", p.read())
    time.sleep_ms(100)
```
## 超声波测距
```python
import time
from openaie import *

us_sensor = ultrasonic(4)
while True:
    us_sensor.measure()  # 触发测量
    time.sleep_ms(100)  # 等待测量完成
    print("distance: %.1fcm\r\n" % us_sensor.read())  # 读取测量结果
    time.sleep_ms(400)
```
## 温湿度读取显示
```python
import time
import lcd
from openaie import th_sensor

th = th_sensor(5)
lcd.rotation(1)
lcd.clear(color=0)
while True:
    info = "temperature: %.1fC  humidity: %d%%" % (th.read_temperature(),
                                                   th.read_humidity())
    print(info)
    lcd.clear(color=0)
    lcd.draw_string(10, 16, info, fc=(0, 0, 255), bc=(0, 0, 0))
    lcd.display()
    time.sleep_ms(1000)
```
## 调速风扇
```python
import time
from openaie import *

p = potentiometer(3)  # 电位器连接 端口3
m = motor_fan(6)  # 电机风扇模块连接 端口6
while True:
    val = p.read()
    print("speed: %d%%" % val)
    m.set(val)
    time.sleep_ms(50)
```
## 语音识别开关灯
```python
import time
from openaie import *

rgb = rgb_led(1)
asr = asr(7) # 语音识别模块连接 端口7
while True:
    res = asr.read() # 读取语音识别结果
    if (res != 0) : # 非 0 为语音识别到唤醒词或命令词
        print("recive cmd: ", res)
        if res == 22 or res == 23 :  # 对应命令词：开灯，打开灯
            rgb.set(0, (50,50,50))
            rgb.display()
        elif res == 25 or res == 26 :  # 对应命令词：关灯，关闭灯
            rgb.set(0, (0,0,0))
            rgb.display()
    time.sleep_ms(100)

```
## 二维码扫描
```python
import time, lcd
from openaie import *

dev = enlighten_com(1)  # 启蒙模块连接到端口1
dev.wait_connect()  # 等待模块链连接
dev.set_mode("qrcode scan")  # 设为二维码扫描模式
lcd.clear(color=(0, 0, 0))
lcd.rotation(0)
lcd.draw_string(10, 10, "二维码扫描", fc=(0, 0, 255), bc=(0, 0, 0))
lcd.display()
while True:
    if dev.request_data("qrcode"):  # 请求二维码扫描结果
        info = dev.read_qrcode_info()
        lcd.clear(color=(0, 0, 0))
        lcd.draw_string(10, 10, info, fc=(0, 0, 255), bc=(0, 0, 0))
        lcd.display()
    time.sleep_ms(200)
```
## 口罩检测
```python
import time, lcd
from openaie import *

dev = enlighten_com(1)  # 启蒙模块连接到端口1
dev.wait_connect()  # 等待模块链连接
dev.set_mode("mask detect")  # 设为口罩检测
tts = tts(7)
lcd.clear(color=(0, 0, 0))
lcd.rotation(0)
lcd.draw_string(10, 10, "口罩检测提醒", fc=(0, 0, 255), bc=(0, 0, 0))
lcd.display()
last_play_time = 0
while True:
    if dev.request_data("mask detect"):  # 请求口罩检测结果
        res = dev.read_detect_info()
        classid = res[4]  # 分类结果
        conf = res[5]  # 可信度
        if (classid == 0):
            info = "请你带好口罩"
            print("没戴口罩，可信度%d%%" % (conf * 100))
        else:
            info = "你好"
            print("戴了口罩，可信度%d%%" % (conf * 100))
        delta = time.ticks_diff(time.ticks_ms(), last_play_time)
        if (delta > 2000):  # 播放间隔大于2S
            tts.play(info)  # 语音播报
            last_play_time = time.ticks_ms()
        lcd.clear(color=(0, 0, 0))
        lcd.draw_string(10, 10, info, fc=(0, 0, 255), bc=(0, 0, 0))
        lcd.display()
    time.sleep_ms(200)
```
## 图像检测分类
```python
import time, lcd
from openaie import *

dev = enlighten_com(1)  # 启蒙模块连接到端口1
dev.wait_connect()  # 等待模块链连接
dev.set_mode("image class")  # 设为图像分类
tts = tts(7)
lcd.clear(color=(0, 0, 0))
lcd.rotation(0)
lcd.draw_string(10, 10, "图像识别分类", fc=(0, 0, 255), bc=(0, 0, 0))
lcd.display()
# 20class 分类的分类标签
label = ("飞机", "自行车", "小鸟", "轮船", "瓶子", "公共汽车", "汽车", "猫", "椅子", "牛",  \
         "餐桌", "狗", "马", "摩托车", "人", "花盆", "羊", "沙发", "火车", "电视")
last_play_time = 0
while True:
    if dev.request_data("image class"):  # 请求二维码扫描结果
        res = dev.read_detect_info()
        info = "识别为: %s, 可信度: %d%%" % (label[res[4]], res[5] * 100)
        delta = time.ticks_diff(time.ticks_ms(), last_play_time)
        if (delta > 2000):  # 播放间隔大于2S
            tts.play(label[res[4]])  # 语音播报
            last_play_time = time.ticks_ms()
        lcd.clear(color=(0, 0, 0))
        lcd.draw_string(10, 10, info, fc=(0, 0, 255), bc=(0, 0, 0))
        lcd.display()
    time.sleep_ms(200)
```
## 颜色识别
```python
import time, lcd
from openaie import *

dev = enlighten_com(1)  # 启蒙模块连接到端口1
dev.wait_connect()  # 等待模块链连接
dev.set_mode("color recognition")  # 设为颜色识别描模式
while True:
    if dev.request_data("red blob"):  # 请求红色色块结果
        x, y, w, h, pixels = dev.read_blob_info()
        print("coordiante: %d, %d pixels: %d" % (x, y, pixels))
    time.sleep_ms(100)
```
## 信息读取显示
```python
import lcd, time, math, _thread
from openaie import rf, bds, get_port_pin
'''
 时区转换 
 @dt: 日期时间 格式[year, month, day, hour, minute, second]
 @timezone: 时区 默认为东8区，即北京时间  
'''


def datetime(dt, timezone=8):
    year, month, day, hour, minute, second = dt[:]
    month_day = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]
    if year % 4 == 0:  # 闰年判断
        month_day[1] = 29
    hour += timezone
    if hour >= 24:
        hour -= 24
        day += 1
        if day > month_day[month - 1]:
            day -= month_day[month - 1]
            month += 1
            if month > 12:
                month = 1
                year += 1
    date_string = "%04d/%02d/%02d" % (year, month, day)
    time_string = "%02d:%02d:%02d " % (hour, minute, second)
    #print(date_string, ' ', time_string)
    return [year, month, day, hour, minute, second]


# 显示屏设置
lcd.set_backlight(50)
lcd.rotation(0)

my_gps = bds(1)

deadline = 0
while True:
    my_gps.update()
    if time.ticks_diff(deadline, time.ticks_ms()) < 0:
        deadline = time.ticks_add(time.ticks_ms(), 500)  # 显示刷新间隔 500ms

        lcd.clear(color=(0, 0, 0))
        lcd.draw_string(72, 5, '卫星定位授时', fc=(0, 0, 255), bc=(0, 0, 0))
        # 显示日期时间
        day, month, year = my_gps.date[:]  # 获取日期（UTC）
        hour, minute, second = my_gps.timestamp[:]  # 获取时间（UTC）
        year, month, day, hour, minute, second = datetime(
            [year + 2000, month, day, hour, minute, second])[:]  # 时区转换
        date_string = "%04d/%02d/%02d" % (year, month, day)
        lcd.draw_string(10, 40, date_string, fc=(0, 0, 255), bc=(0, 0, 0))
        time_string = "%02d:%02d:%02d " % (hour, minute, second)
        lcd.draw_string(110, 40, time_string, fc=(0, 0, 255), bc=(0, 0, 0))
        # 卫星信息
        lcd.draw_string(10,
                        75,
                        '可见卫星: %s 颗' % my_gps.satellites_in_view,
                        fc=(0, 0, 255),
                        bc=(0, 0, 0))
        lcd.draw_string(10,
                        95,
                        '使用卫星: %s 颗' % my_gps.satellites_in_use,
                        fc=(0, 0, 255),
                        bc=(0, 0, 0))
        # 位置
        longitude = my_gps.longitude[0]
        latitude = my_gps.latitude[0]
        lcd.draw_string(10,
                        115,
                        '经度: %s' % longitude,
                        fc=(0, 0, 255),
                        bc=(0, 0, 0))
        lcd.draw_string(10,
                        135,
                        '纬度: %s' % latitude,
                        fc=(0, 0, 255),
                        bc=(0, 0, 0))
        lcd.draw_string(10,
                        155,
                        '海拔: %d m' % my_gps.altitude,
                        fc=(0, 0, 255),
                        bc=(0, 0, 0))
        lcd.draw_string(10,
                        175,
                        '速度: %.2f km/h' % my_gps.speed[2],
                        fc=(0, 0, 255),
                        bc=(0, 0, 0))
        lcd.display()
```