# Chromium多进程模型

## Browser进程

## Renderer进程

### Compositor Thread
这个线程既负责接收浏览器传来的垂直同步信号，也负责接收OS传来的用户交互，比如滚动、输入、点击、鼠标移动等等。
* **直接处理** Compositor Thread会直接负责处理这些输入，然后转换为对layer的位移和处理，并将新的帧直接commit到GPU Thread，从而直接输出新的页面。
* **唤醒Main Thread** 让Main Thread去执行JS、完成重绘、重排等过程，产出新的纹理，然后Compositor Thread再进行相关纹理的commit至GPU Thread，完成输出

#### 垂直同步
为了平衡显示器的刷新率和显卡的帧速

### Main Thread
某段JS的执行、Recalculate Style、Update Layer Tree、Paint、Composite Layers等等