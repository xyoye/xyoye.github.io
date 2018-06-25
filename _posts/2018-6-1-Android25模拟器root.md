# Android25 模拟器Root #

为了在模拟器上研究一个xposed项目不得不尝试对模拟器进行root，网上看了一些教程，在尝试之后发现总是没有权限去写入su文件，后面在stackflow看了一个回答，结合前面的经历，终于搞定了。

## 一、工具

需要准备的工具：

1、[SuperSU app 2.82](https://play.google.com/store/apps/details?id=eu.chainfire.supersu)

2、[SR5-SuperSU-v2.78.zip (密码：6phd)](https://pan.baidu.com/s/1tgZCmefkPGSjq0kTP4ZF8g) 
       
3、Android Studio 及 Android7.1.1的模拟器

4、配好的adb环境变量 

## 二、流程

### 第一步

通过Android Studio打开模拟器，在模拟器中安装第一个软件SuperSu。
这个SuperSu就是用来授权的软件，安装后打不打开都无所谓，打开也不能用先。

### 第二步

到你的的sdk安装目录下打开tools文件夹，按住shift+鼠标右键选择：在此处打开命令窗口，打开终端后输入：emulator.exe –avd {你的模拟器名称} –writable-system

例：emulator.exe –avd A25 –writable-system

![](https://i.imgur.com/kbIJvBP.png)

输入之后显示如下，之后就不用管这个了终端了：

![](https://i.imgur.com/KxpZky8.png)

### 第三步
解压第二个文件后找到你模拟器对应的内核的文件夹，比如我的是x86的模拟器，就进入x86文件夹。复制路径。 打开Android Studio的Terminal。

输入：

	adb root
	adb remount

结果：

![](https://i.imgur.com/Jzh8O6z.png)

**注意：输入后必须显示remount succeeded，否则没法进行下去了。要是失败了，点击模拟器电源键重启模拟器**

### 第四步
输入：
	
	adb -e push 上面你复制的路径\su.pie /system/bin/su

结果：

![](https://i.imgur.com/MavZqVF.png)

**注意：如果出现说不能写入之类的情况，请重启。**

另外经过测试，貌似不能写入是因为命令输入的太久了，重启模拟器后再快速输入一遍这三条命令就可以了

### 第五步

输入：

	adb -e shell
	su root
	cd /system/bin
	chmod 06755 su
	su --install
	su --daemon
	setenforce 0

结果：没有结果，就是正确的结果！

如果输入完setenforce 0都没有问题的话，那么恭喜你，root成功了！要是输入过程中报错了，删了当前模拟器重来吧。

## 检测权限
最后，检验一下root权限，我的检验方法是下载一个es文件管理器，打开root工具箱，如果弹窗询问是否授予root权限，那就是OK了。

##
参考链接：[Root Android virtual device with Android 7.1.1](https://android.stackexchange.com/questions/171442/root-android-virtual-device-with-android-7-1-1)

作者：xyoye  
联系方式：yeshao1997@outlook.com  
时间：2018.6.1