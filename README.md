
> 2017 年 12 月 28 日下午，微信发布了 6.6.1 版本，加入了「小游戏」功能，并提供了官方 DEMO「跳一跳」。


>自己也是上瘾，玩了一百多局，也只有得到189分，后来看到网上一系列“教你用Python玩跳一跳”的文章，故决定写一篇经验贴用**最简单的方式**实现微信跳一跳游戏。

### **开始前说明**

> 为了环境统一和成功实现，本文使用安卓（蓝叠）虚拟机实现。本文主要以：`Android` + `Windows` 实现，Mac下同理也可以通过`Android` + `MacOS`实现。

> 希望大家不要把分数刷太高，容易没朋友的。分数太高被会查封，小心被微信官方团队问候。

>  **微信跳一跳QQ交流群：  657588688**




###  **环境依赖安装**

- **Python 2.7**
    * 点击[这里](https://www.python.org/ftp/python/2.7.14/python-2.7.14.msi)，下载`python-2.7.14.msi`一路next到成功即可
    * 打开控制台，输入：`python --version`，出现`Python 2.7.xx`字样说明安装成功

- **Python依赖**（依赖包在`requirements.txt`中）
    *   依赖包安装：`pip install -r requirements.txt`


- **手机或者模拟器**（这里推荐使用`蓝叠模拟器`，点击[这里](http://202.114.96.192/aliosscdn.bluestacks.cn/package/BlueStacksGPSetup.exe)直接下载，蓝叠[官网](http://www.bluestacks.cn/)，建议不要使用夜神模拟器，无法使用adb连接）

- **adb**（位于 Android SDK 中：`platform-tools/adb`）
    *   安装 [Universal ADB Drivers](https://adb.clockworkmod.com/) 后，请在 环境变量 里将 adb 的安装路径保存到 PATH 变量里，确保 `adb` 命令可以被识别到。
    *   或者你也可以安装完整版Android SDK包 | 你需要安装好java环境



### **手机和模拟器说明**

  * 请将安卓手机的 USB 调试模式打开，设置》更多设置》开发者选项》USB 调试，如果出现运行脚本后小人不跳的情况，请检查是否有打开“USB 调试（安全模式）”
  * 根据大家反馈：1080 屏幕距离系数 **1.393**，2K 屏幕为 **1**，建议使用1080p
  * 添加部分机型配置文件，可直接复制使用
  * 如果使用蓝叠模拟器：设置分辨率为：`1080 * 1920`（竖屏），如果是`1920 * 1080`（横屏） 则模拟器的截图是横屏，导致识别出错



### **原理说明**

> （代码实现见：`wechat_jump_auto.py`，以下只是对于代码实现原理的介绍）

* 将手机点击到《跳一跳》小程序界面；
* 用 ADB 工具获取当前手机截图，并用 ADB 将截图 pull 上来
```shell
adb shell screencap -p /sdcard/autojump.png
adb pull /sdcard/autojump.png .
```
<div align=center>
<img src="https://raw.githubusercontent.com/frank-lam/weixin_jump/master/images/jump_trace.png"/>
</div>


* 计算按压时间

  * 如果能精确测量出起始和目标点之间测距离，就可以估计按压的时间来精确跳跃

  * **手动版**：用 Matplotlib 显示截图，用鼠标点击起始点和目标位置，计算像素距离；
  * **自动版**：靠棋子的颜色来识别棋子，靠底色和方块的色差来识别棋盘；

<div align=center>
<img src="https://raw.githubusercontent.com/frank-lam/weixin_jump/master/images/jump_distance2.png"/>
</div>

* 用 ADB 工具点击屏幕蓄力一跳；

```shell
    adb shell input swipe x y x y time(ms)
```



### **安卓手机（模拟机）操作步骤，到这里才正式开始**


- 1.安卓手机打开 USB 调试，设置》开发者选项》USB 调试

- 2.电脑与手机 USB 线连接，确保执行`adb devices`可以找到设备 ID。**`tips`**：如果打开蓝叠模拟器，无法找到你的设备，如：`emulator-5554   device`，请执行命令：`adb connect localhost:5555`，蓝叠默认是`localhost:5555`。这时候`adb devices`就可以找到设备
- 3.界面转至微信跳一跳游戏，点击开始游戏

- 4.运行`python wechat_jump_auto.py`，如果手机界面显示 USB 授权，请点击确认。或者也可以直接打开目录中的`wechat_jump_auto.py`文件，可直接运行

- 注意：请按照你的手机分辨率从`./config/`文件夹找到相应的配置，拷贝到 xxx.py 同级目录`./config.json`（如果屏幕分辨率能成功探测，会直接调用 config 目录的配置，不需要复制，记住要把配置文件重命名为：`config.json`）。


### **实验结果**
<div align=center>
<img src="https://raw.githubusercontent.com/frank-lam/weixin_jump/master/images/result.jpg"/>
<img width="400" src="https://raw.githubusercontent.com/frank-lam/weixin_jump/master/images/result_rank.png"/>
<div>



### **联系作者**
> 事实证明，机器人比人更会玩儿游戏。哈哈，千万不要刷高分，否则会被揍，请稳步刷分。

> **项目地址**：https://github.com/frank-lam/weixin_jump

> **CSDN博客**：http://blog.csdn.net/u012104219

> **Email**：frank_lin@whu.edu.cn

> 如果你觉得不错的话，不妨打赏一下，这样我就有更大的动力去完善它，优化它。


<img src="https://raw.githubusercontent.com/frank-lam/weixin_jump/master/images/tipping.bmp"/>
