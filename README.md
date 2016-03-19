# port
minicom is too complicated.  This command talks to a serial port.

use this module to read data from pm2.5 detect hardware from serial port.

I bought from:
https://detail.tmall.com/item.htm?id=43900458700&spm=a1z09.2.0.0.xFaBBB&_u=jgkdikc670

#How to Use with the code
#####1.type ./port /dev/tty.wchusbserial1420 -s 9600 in the terminal, tty.xxx means your serial port(here I use a hardware module which can transform the datathrough usb)
#####2.shows the result like this:
#####➜  port git:(master) ✗ ./port /dev/tty.wchusbserial1420 -s 9600
#####(Type ~. or !. to exit, or ~b to send BREAK)
#####(Line Status: DTR, RTS)
170
170
170
170
169
169
169
169
169
170
170
170
170
170
170
170
171
170
171
not good air quality!
#pm2.5 hardware module data configure
这里随机选了一组数据进行计算，如上图红色框里的数据： AA C0 C1 04 89 09 00 00 57 AB
具体含义：
AA----报文头

C0----指令号，客户开发产品时，看到接收到有CO，即表示是由PM2.5传感器输出的信号

C1----PM2.5低字节

04----PM2.5高字节

89----PM10低字

09----PM10高字节

00----保留位，暂未用，可以用做传感器的ID（在多台传感器同时使用时，以便于区分）

00----保留位，暂未用，可以用做传感器的ID（在多台传感器同时使用时，以便于区分）

57----校验和，即C1+04+89+09+00+00=157（即0X0157）

省略高字节，保留低字节

AB----报文尾

因为输出的是16进制数据，请转换成10进制数进行计算。

PM2.5值的计算:C1 04

      低字节  C1:  12*16+1=193

      高字节  04:    0*16+4=4

          ((PM2.5 高字节*256) + PM2.5 低字节)/10

         （4*256+193）/10=121.7ug/m3

 

PM10值的计算:89 09

     低字节  89:   8*16+9=137

      高字节  09:   0*16+9=9

          ((PM10高字节*256) + PM10 低字节)/10 

                       (9*256+137)/10=244.1ug/m3

校验和：57     C1+04+89+09+00+00
