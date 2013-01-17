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

##发布到ipad
1. 新建Mobile应用(testM)
2. 修改app.xml(testM-app.xml)中的<id>项目，与申请的apple应用证书一样
3. 导出发行版->数字签名（证书、密码、配置文件）
