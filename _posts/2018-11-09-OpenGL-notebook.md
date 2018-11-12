---
layout: post
title:  OpenGL 问题汇总
date:   2018-11-09 15:00:00 +0800
categories: document
tag: 教程
---

* content
{:toc}

OpenGL 问题汇总			{#yungentie}
====================================

1). SurfaceTexture的updateTexImage必须与渲染thread在同一线程中

  SurfaceTexture objects may be created on any thread.  {@link #updateTexImage} may only be
  called on the thread with the OpenGL ES context that contains the texture object.  The
  frame-available callback is called on an arbitrary thread, so unless special care is taken {@link
  #updateTexImage} should not be called directly from the callback.

  比如采用这样的方式：
  GLSurfaceView->setRender->onSurfaceCreated回调方法中构造一个SurfaceTexture对象，然后设置到Camera预览中->SurfaceTexture中的回调方法onFrameAvailable来得知一帧的数据准备好了->requestRender通知Render来绘制数据->在Render的回调方法onDrawFrame中调用SurfaceTexture的updateTexImage方法获取一帧数据，然后开始使用GL来进行绘制预览。


  2).设置GLSurfaceView透明
  https://www.jianshu.com/p/4e5053df19a3
  实现步骤：
  1.在调用GLSurfaceView.setRenderer()之前调用：
  GLSurfaceView.setEGLConfigChooser(8, 8, 8, 8, 16, 0);
  GLSurfaceView.getHolder().setFormat(PixelFormat.TRANSLUCENT);
  GLSurfaceView.setZOrderOnTop(true);
  
  2.在GLSurfaceView.Renderer的onSurfaceCreated方法里，把背景的透明度设为0：
  GLES20.glClearColor(0,0,0,0);
  
  作者：_Ricky_
  链接：https://www.jianshu.com/p/4e5053df19a3
  來源：简书
  简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。


  3).视频播放完毕之后的reset过程
  MediaCodec中使用的SurfaceTextureb在每次解码完成后会被release，主意在下次解码开始前重新创建

  

模板测试页			{#yungentie}
====================================

看了下历史记录，在6月8号切换的云跟帖，未到一个月，7月6号，云跟帖也宣告结束了。

![/styles/images/abandon-comments/yungentie-abandon.png]({{ '/styles/images/abandon-comments/yungentie-abandon.png' | prepend: site.baseurl  }})

撇开第三方评论系统的盈利与否不谈，我好像发现了什么不得了的事情，就是我用什么，就死什么。

我用Disqus，被墙了；用多说，关停了；用云跟帖，挂了。


选择 - 回归			{#back-to-begin}
====================================

当看到云跟帖要关停的消息的时候，首先想的是还有什么替代方案。

考虑了好久，觉得对现在的第三方评论系统都没什么信任感，基于我用什么死什么，我也不想再祸害一个新的好项目了。并且细细想来，我写这个博客的初衷是记录一些我在工作或者学习中遇到或者学到的知识，随着年岁的增长，记忆力直线下降，所以只是拿博客当作一个在线的技术日记记录工具，供我能够随时方便地找到那些我已经遗忘细节的知识点，并没有想过什么著书立传之类的事情。

能帮到别人固然是好的，不过归根结底是给自己一个方便查阅的平台。

所以最后的决定是彻底放弃评论功能。如果有人在看我的博客的时候有什么问题，依然可以到github开issue来提问。由于去github开issue相对还是有点技术含量(全球最大的同性交友平台还是技术人员的主场)，这样还屏蔽掉了大部分的灌水或者垃圾评论。

所以，别了，评论功能！
