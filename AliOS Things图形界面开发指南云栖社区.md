---
title: AliOS Things图形界面开发指南
date: 2018-04-18 19:49:30
categories: "开发"
tags:
	- 酷玩
	- Linux
	- 鼠标
	- GPU
	- 穿戴设备

---

# 简介 #

物联网设备开发过程中，嵌入式GUI（用户图形界面）的开发是一个重要的组成部分。许多智能设备如智能家电、智能手表、智能仪表上都会涉及到GUI开发。AliOS Things集成开源图形库littlevGL，可以在linux上进行图形界面开发。开发完成后将代码添加到相应的工程并完成显示和输入设备驱动的对接，程序即可在相应的硬件上运行，方便用户进行嵌入式GUI开发。

littlevGL是一个开源的嵌入式图形库，采用C语言开发，使用MIT协议，并在持续更新中。该图形库支持常用的控件，如按钮、列表、滑块、选择框、仪表盘、键盘、波形等。并支持触摸、鼠标、键盘等多种输入方式。其官方网站为：https://littlevgl.com。

# linux模拟开发步骤 #

**1、环境安装**

a、按照AliOS Things Linux Environment Setup安装基本环境。

b、按照如下命令安装SDL2图形库。

sudo apt-get install libsdl2-2.0:i386

sudo apt-get install libxkbcommon-dev:i386

sudo apt-get install libmircommon-dev:i386

sudo apt-get install libmirclient-dev:i386

sudo apt-get install libegl1-mesa-dev:i386

sudo apt-get install libglib2.0-dev:i386

sudo apt-get install libpulse-dev:i386

sudo apt-get install libsdl2-dev:i386

**2、模拟运行**

编译运行命令：aos make littlevgl\_simulate@linuxhost

编译通过之后会自动运行生成的可执行文件，界面如下

![AliOS Things图形界面开发指南][AliOS Things]

用户可以自行添加应用代码，开发完成后将代码添加到相应的工程并完成显示和输入设备驱动的对接，程序即可在相应的硬件上运行。

example/littlevgl\_simulate/lv\_examples目录下包含demo程序以及各个控件使用的示例程序，可参考进行界面开发。

# AliOS Things开发版开发步骤： #

在starterkit的开发版上已经移植littlevgl，用户直接运行命令即可编译GUI工程。

编译命令：aos make littlevgl\_starterkit@starterkit

编译完成后下载到starterkit即可。

![AliOS Things图形界面开发指南][AliOS Things 1]

# 其他CPU开发步骤： #

**1、显示驱动实现**

a、使用内部缓冲区（lv\_conf.h的LV\_VDB\_SIZE > 0）

必须实现如下函数，其功能为对一片矩形区域填充颜色�����注意该函数的最后必须调用lv\_flush\_ready()函数。

> /\*Write the internal buffer (VDB) to the display. 'lv\_flush\_ready()' has to be called when finished\*/
> 
> void my\_disp\_flush(int32\_t x1, int32\_t y1, int32\_t x2, int32\_t y2, const lv\_color\_t \* color\_p)
> 
> \{
> 
> /\*TODO Copy 'color\_p' to the specified area\*/
> 
> /\*Call 'lv\_fluh\_ready()' when ready\*/
> 
> lv\_flush\_ready();
> 
> \}

b、使用硬件加速（USE\_LV\_GPU = 1 且使用内部缓冲区）

必须实现如下函数：

> /\*Blend two memories using opacity (GPU only)\*/
> 
> void my\_mem\_blend(lv\_color\_t \* dest, const lv\_color\_t \* src, uint32\_t length, lv\_opa\_t opa)
> 
> \{
> 
> /\*TODO Copy 'src' to 'dest' but blend it with 'opa' alpha \*/
> 
> \}
> 
> /\*Fill a memory with a color (GPU only)\*/
> 
> void my\_mem\_fill(lv\_color\_t \* dest, uint32\_t length, lv\_color\_t color)
> 
> \{
> 
> /\*TODO Fill 'length' pixels in 'dest' with 'color'\*/
> 
> \}

c、使用缓区

必须实现如下函数：

> /\*Fill an area with a color on the display\*/
> 
> void my\_disp\_map(int32\_t x1, int32\_t y1, int32\_t x2, int32\_t y2, const lv\_color\_t \* color\_p)
> 
> \{
> 
> /\*TODO Copy 'color\_p' to the specified area\*/
> 
> \}
> 
> \*Write pixel map (e.g. image) to the display\*/
> 
> void my\_disp\_fill(int32\_t x1, int32\_t y1, int32\_t x2, int32\_t y2, lv\_color\_t color)
> 
> \{
> 
> /\*TODO Fill the specified area with 'color'\*/
> 
> \}

**2、输入驱动实现**

a、触摸或者鼠标等点输入设备

必须使用如下函数，获取点坐标

``````````
bool my_input_read(lv_indev_data_t *data) { data->point.x = touchpad_x; data->point.y = touchpad_y; data->state = LV_INDEV_EVENT_PR or LV_INDEV_EVENT_REL; return false;  /*No buffering so no more data read*/}
``````````

b、键盘设备

必须使用如下函数：

``````````
bool keyboard_read(lv_indev_data_t *data) { data->key = last_key();  if(key_pressed()) { data->state = LV_INDEV_EVENT_PR; } else { data->state = LV_INDEV_EVENT_REL; } return false; /*No buffering so no more data read*/}
``````````

**3、初始化**

a、将framework/GUI/littlevGL目录下的文件添加到工程。

b、根据需要配置lv\_conf.h中相应的宏定义。

c、调用lv\_init()初始化littlevGL。

d、初始化显示和输入（键盘、鼠标、触摸等）设备。

e、调用lv\_disp\_drv\_init初始化显示驱动，调用lv\_disp\_drv\_register注册显示驱动。调用lv\_indev\_drv\_init初始化输入驱动，调用lv\_indev\_drv\_register注册输入驱动，示例代码见附录。

f、在时钟中断中调用lv\_tick\_inc(1)，为littlevGL提供心跳。

g、创建一个低优先级任务，在其中重复调用lv\_task\_handler函数，进行图像的刷新和输入事件的响应。

**4、APP编写**

用户可以自行添加相应的应用代码。

# 其他 #

AliOS Things也支持STemwin，在starterkit的开发版上已经移植STemwin，用户直接运行命令即可编译GUI工程。

编译命令：aos make starterkitgui@starterkit

# 附录 #

驱动初始化和注册示例代码如下：

``````````
lv_disp_drv_t dis_drv;lv_indev_drv_t indev_drv;void lvgl_drv_register(void){ lv_disp_drv_init(&dis_drv); dis_drv.disp_flush = my_disp_flush; dis_drv.disp_fill = my_disp_fill; dis_drv.disp_map = my_disp_map; lv_disp_drv_register(&dis_drv); lv_indev_drv_init(&indev_drv); indev_drv.type = LV_INDEV_TYPE_POINTER; indev_drv.read = my_input_read; lv_indev_drv_register(&indev_drv); }void my_disp_flush(int32_t x1, int32_t y1, int32_t x2, int32_t y2, const lv_color_t * color_p){ int32_t x = 0; int32_t y = 0; for (y = y1; y <= y2; y++) /*Pick the next row*/ { for (x = x1; x <= x2; x++) /*Pick the a pixel in the row*/ { BSP_LCD_DrawPixel(x,y, color_p); color_p++; } } lv_flush_ready();}void my_disp_fill(int32_t x1, int32_t y1, int32_t x2, int32_t y2, lv_color_t color){ int32_t i =0; int32_t j =0;  for (i = x1; i <= x2; i++) { for (j = y1; j <= y2; j++) { BSP_LCD_DrawPixel(i,j, color.full); } }}void my_disp_map(int32_t x1, int32_t y1, int32_t x2, int32_t y2, const lv_color_t * color_p){ int32_t i =0; int32_t j =0;  for (i = x1; i <= x2; i++) { for (j = y1; j <= y2; j++) { BSP_LCD_DrawPixel(i,j, color_p->full); color_p++; } }}bool my_input_read(lv_indev_data_t *data) { __IO TS_StateTypeDef ts; BSP_TS_GetState((TS_StateTypeDef *)&ts); ts.touchX[0] = TouchScreen_Get_Calibrated_X(ts.touchX[0]); ts.touchY[0] = TouchScreen_Get_Calibrated_Y(ts.touchY[0]);  if((ts.touchX[0] >= 240) ||(ts.touchY[0] >= 240) ) { ts.touchX[0] = 0; ts.touchY[0] = 0; }  if((TS_State_cur.Pressed != ts.touchDetected )|| (TS_State_cur.x != ts.touchX[0]) || (TS_State_cur.y != ts.touchY[0])) { TS_State_cur.Pressed = ts.touchDetected; if(ts.touchDetected) { TS_State_cur.x = ts.touchX[0]; TS_State_cur.y = ts.touchY[0]; data->point.x = TS_State_cur.x; data->point.y = 240 - TS_State_cur.y;  if(TS_State_cur.Pressed == ts.touchDetected) { data->state = 1; } else { data->state = 0; }  } else { TS_State_cur.x = 0; TS_State_cur.y = 0;  } }  return false; /*No buffering so no more data read*/ }
``````````


[AliOS Things]: /pro/os/crawler/3YJ6-BFAI-YE7F.jpg
[AliOS Things 1]: http://p3.pstatp.com/large/pgc-image/15240519046286ac9a7bfc3
 *  **原文作者：** 云栖社区
 *  **原文链接：** https://www.toutiao.com/item/6545754229049393671/
 *  **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0][] 许��协议。转载请注��出处。