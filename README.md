## 使用方式
切换到工程主目录，执行以下命令即可。（看网速，大约需要10到60分钟）
* git submodule init
* git submodule update

## 简介
原有的AOSP项目中包含了几百个项目，总量在190G左右，但是大多数对APP开发者来说是无用。
所以本项目只同步了AOSP中几个对APP开发者来说最重要的核心项目（8个G左右），方便开发者查阅源码。
包含以下几个库：
* frameworks/base
* frameworks/native
* frameworks/multidex
* libcore
* art
* system/core


## 详细介绍
研究frameworks层的源码，如果只看Android/sdk/sources下的源码，你会发现是远远不够的。
比如你会发现WindowManagerImpl.java的addView方法如下：

<p>@Override </p>
<p>public void addView(View arg0, android.view.ViewGroup.LayoutParams arg1) {</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;// pass</p>
<p>}</p>
而实际的WindowManagerImpl.java中addView方法如下：
<p>
<p>@Override</p>
<p>public void addView(@NonNull View view, @NonNull ViewGroup.LayoutParams params) {</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;applyTokens(params);</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mGlobal.addView(view, params, mContext.getDisplayNoVerify(), mParentWindow,</p>
<p>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;mContext.getUserId());</p>
<p>}</p>
   
同样的还是DexClassLoader等等。sources中代码并不是android中真正执行的代码，而仅仅只是一个参考。

另外，诸如surfaceFlinger.cpp，DexClassLoader.java等代码，其实并不是base这个项目中，而是在native，libcore这些项目中，所以并不能只同步base这一个项目。
具体项目对应的关系如下：
* frameworks/base:app的核心库，APP进程中使用到的所有java类和native类，SystemServer进程（AMS，WMS就属于这个进程）中使用到的所有java类和native类，以及SurfaceFlinger进程中用到的java类，都在这个项目中。
* frameworks/native:SurfaceFlinger进程中用到的native类,底层服务（诸如蓝牙，电源管理，USB连接等等）的native实现类，都在这个项目中。
* frameworks/multidex:顾名思义，主要是关于multidex的类。
* libcore:主要包含DexClassLoader等ClassLoader类，JSON类，VMRuntime类等等。
* art:ART虚拟机的实现，odex的优化实现等等。
* system/core:主要是安卓第一个进程：init进程相关的内容。


## 备注
目前使用的是submodule的方式进行更新的，几个子项目使用了如下的仓库镜像来源。
* git clone https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/frameworks/base
* git clone https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/frameworks/native
* git clone https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/frameworks/multidex
* git clone https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/libcore
* git clone https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/art
* git clone https://mirrors.tuna.tsinghua.edu.cn/git/AOSP/platform/system/core

对应AOSP的仓库地址，相关的仓库地址来源于：https://android.googlesource.com/
* git clone https://android.googlesource.com/platform/frameworks/base
* git clone https://android.googlesource.com/platform/frameworks/native
* git clone https://android.googlesource.com/platform/frameworks/multidex
* git clone https://android.googlesource.com/platform/libcore
* git clone https://android.googlesource.com/platform/art
* git clone https://android.googlesource.com/platform/system/core

## 后续改进
后续希望能够改成clone了一个主项目，6个子项目也都存在，并且可以单独提交更新的那种方式。
如果谁有这方面的经验，欢迎指教。


