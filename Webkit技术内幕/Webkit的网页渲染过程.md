# Webkit的渲染过程
用户输入URL，浏览器经过一系列处理后返回可视化的图像，这中间浏览器做了什么，就是本章需要讲解的内容。我们把从输入URL开始到构建DOM树的过程称为加载过程，把生成DOM树后到生成可视化图像称为渲染过程。
## 加载
URL输入 》》》 资源加载器加载 》》》HTML解释器 》》》JavaScript引擎 》》》DOM树
PS:我们在接收到HTML网页后，网页被交给HTML解释器转换为一系列词语(token)后构建节点，形成DOM树，而如果节点是JavaScript代码的话JavaScript引擎会被调用解释并执行，可能会修改DOM的结构，如果节点需要其他资源的话，资源加载器会被调用去获取资源，目前大多数资源都是异步获取所以不会影响当前DOM树的创建，但JavaScript资源却会暂停DOM的创建，直到资源加载完毕并执行完所有同步代码。这点就显示出DOMContent和onload的不同。
## 渲染
DOM树 》》》 RenderObject树 》》》RenderLayer树 》》》绘图上下文 
CSS文件加载完后被CSS解释器解释后附上了DOM树就形成了RenderObject树，RenderObject树被创建后Webkit根据网页层次生成RenderLayer树，同时构建一个虚拟的绘图上下文，绘图具体实现类依赖2D/3D图形库生成最终的图形。