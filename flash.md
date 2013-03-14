##Flash优化
- 善用profile工具（对调用频繁的函数内联，对开销大的算法优化,在flash builder 4.7中加入了内联支持）
- UI中如果有SWF动画，隐藏时必须先停止播放(可以按照debug版本的flash player10观察重绘区，比flash player11更能发现问题)

##Stage3D优化

###优化
- 尽量减少API调用
- 尽量合并材质，特别是减少材质upload次数
- 尽量合并顶点及索引buffer，1次update只上传一次buffer数据
- setProgram/setProgramConst**/setTexture等开销较小，可以大胆使用
- shader中访问某一个分量时，必须使用xyzw，使用rgba会修改整个4分量

###功能
- Matrix变化可以直接转换为顶点
- 对颜色的处理放到ColorMatrixFilter中
- 对特定区域设置特殊的alpha进行shader变色
- 默认对所有物体的混合模式是以 ONE，对需要进行特殊颜色转换的使用SOURCE_ALPHA

##Flash用AIR转移动平台注意事项
###1. JSON 和 Flash SDK 版本

页游用的Flash SDK版本比较低，而手机一般用的SDK至少是AIR 3.5，属于非常新的SDK了。

导致的问题是，Adobe在某一版SDK中把JSON实现和公开方法名字都改了，不叫encode和decode了。

目前页游里包含有JSON源码，是老版本，导致与SDK自带的JSON冲突。

解决方法：

方法一：升级SDK

方法二：将页游自带的JSON类改名

###2. Loader

虽然页游几乎没怎么改就能在安卓上跑起来了，但还是要把LoaderContext.allowCodeImport设成true，否则恐怕出问题。这个设置对页游没影响。

###3. 加监听顺序

比如在函数V_Login_View.loadSwf里

loader.load(new URLRequest(GameConfig.va_pb_loginResPath+"libraryswf/login.swf"), loaderContext);

loader.contentLoaderInfo.addEventListener(Event.COMPLETE , function onCom(e:Event):void

在load之后加监听。我不是100%肯定这个有问题，但在手机如果发生了load马上完成，那么后面的监听就会失效。

###4. 资源加载

资源加载的代码越集中越好，这样手机上有可能改用文件操作而不是URLLoader。

###5.外部SWF不要有代码

代码都集中在主SWF里。外部加载的SWF不要有代码，否则IOS上会很麻烦。

###6. 内存释放

移动设备上基本要尽量保存内存占用最小，所以临时分配的内存尽量在使用后释放掉。

所以即使页游不释放那些可释放的内存来增加响应速度，如果能把模块设计的干净，可以完全释放，那是最好。

比如一个面板关闭后，页游也许会继续引用它，但手机上如果能把引用设成null儿导致这个面板被垃圾回收，那是最好的了。这只是简单的例子，但以前在帝国项目中，很多面板都非常难释放。下面几点是一些导致难释放的原因.

* 避免向全局对象加监听，如果加监听了，也要在关闭时移除监听。帝国很多面板在构造函数里通过静态函数向一个第三方的NResponder添加监听，然后再没移除，导致面板无法释放。这对帝国页游没问题，因为它本来就不释放，但到手机上是很大问题。
* 避免全局静态对象，多用依赖注入（DI）和反转控制（IOC）。
* 多用弱引用。加监听时如果能用弱引用则用。当引用对象时，也尽量用Dictionary模拟的弱引用，这样可以避免在复杂情况下出现图状的循环引用。

###7. 性能优化

* 拖拽。不要用Sprite的startDrag，而要在每帧update里自己调整位置。	原因是在一帧里会有多个鼠标移动事件，这样startDrag会产生多个重绘事件。而在每帧update里自己跳帧位置，一帧只产生一个重绘事件，减少掉帧。
* 尽量用帧update而不是Timer。Timer是比较耗费资源的。多个Timer同时存在更加浪费  。我们可以在帧update里自己计算过去的时间，一旦达标则触发相应事件。
* 尽量减少监听ENTER_FRAME事件。ENTER_FRAME也比较耗费性能。理想讲一个游戏最好只有一个ENTER_FRAME，然后在此事件处理里分别调用需要更新的函数。

##Flash发布到iOS步骤
1. 新建Mobile应用(testM)
2. 修改app.xml(testM-app.xml)中的<id>项目，与申请的apple应用证书一样
3. 导出发行版->数字签名（证书、密码、配置文件）

##Debug
1. 使用本地信任文件
在Windows XP操作系统中，当前用户的本地信任文件路径为：
C:\Documents and Settings\[你的用户名]\Application Data\Macromedia\Flash Player\#Security\FlashPlayerTrust\flashbuilder.cfg
