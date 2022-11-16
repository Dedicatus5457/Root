# 卡刷

有的Recovery作者附带了刷入脚本，如果你是这种情况，请按照脚本指南操作。

如果你不是，下面是关于卡刷和使用第三方 Recovery的通用方法。

## 准备

- 电脑
- 一台解锁 Bootloader 的安卓 ？ 以上的手机
- 一根良好的数据线
- 卡刷包
- Magisk.zip(Root需选) 

### ADB

!!! warning
    线必须是原装线或者质量靠谱的线，否则会出现 `USB Error` ！

如果你没有使用 MTK ，请通过`adb devices`命令再次确认已经连上手机。

!!! tip "找不到adb？？"
    如果你不想配置ADB全局环境的话，以后执行 Adb 命令需要在工具目录下，按住 Shift 右键鼠标打开终端，命令替换为`.\adb`或者`.\fastboot`，如果需要配置全局环境，请按照[这篇文档](https://www.sunzn.com/2018/08/02/Windows-10-%E4%B8%8B%E9%85%8D%E7%BD%AE-ADB-%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F/)配置。


💡 给手机刷入 Recovery 其实也不需要电脑，用另一部手机借助 [Termux](https://f-droid.org/zh_Hans/packages/com.termux/) 也是同样可以的。在 Termux 配置好 ADB 环境后，照上述步骤刷入即可。安装 Termux 的那部手机无需 ROOT。

### 准备文件

以 `OrangeFox` 为示例，下载并打开 `OrangeFox` 压缩包，解压其中的 `recovery.img` 到任意一个位置。

压缩包是为了在有第三方 Recovery 的情况下刷入/更新 OrangeFox ，但是如果你没有，需要解压其中的 `recovery.img` 来使用 `fastboot` 刷入。

如果你选择使用 LR.Team 定制版 TWRP，可以选择一键刷入版。

### 提前说明

* 新款联发科机型必须关闭AVB2.0校验

否则任何对系统的修改都会导致卡在 Fastboot。涉及的机型包括但不限于红米6，6A，9，9A，红米Note8Pro，红米10X4G，5G，红米K30U，小米Play，以及之后的联发科机型。至于AVB2.0怎么关闭，不同机型不同安卓版本方法不用，可以在TWRP里找这个功能，或者找刷过的大佬问方法。

* 红米3S进入Recovery模式的方法是长按三键

## 刷机

手机关机，**长按 音量减键 + 电源键** 进入 `FASTBOOT` 模式。用数据线把手机和电脑连接起来。

然后打开终端，输入 `fastboot flash recovery 解压的 recovery.img 的文件地址` （或者 `.\fastboot`？） 
>这一步需要按照上面的指南配置好 ADB 环境。

执行输出

```
sending recovery OKAY（传输Recovery）
writing recovery OKAY（刷入Recovery）
finished.
```

执行完毕后**长按 音量加键 + 电源键**，出第一屏即一秒左右可松手，进入 `Recovery`模式，你也可以使用 `fastboot reboot recovery`来进入 `Recovery`模式。

一般情况下，会出现一个英文界面，从上往下依次写着 `Mount` `Dectypt` ，这是让你解密 Data 。

解密DATA分区后，才能刷入第三方 ROM ，可以理解为是对官方系统的保护。但解密 DATA 会格式化数据。

!!! danger
    解密完成后，DATA 被清空，包括内置卡。
    
    **请先备份好手机上的一切重要资料，包括手机存储/内置卡/外置卡上重要数据**

在界面输入你的锁屏密码，输入完后点击右下角确认。经过十秒左右秒钟的解密后，不出意外会显示欢迎界面，点击屏幕下方勾即可。然后就是挂载 `System` 的界面，滑动下方滑块进入主界面。

!!! info
    💡 如果你是卡刷官方包后，不做任何操作是肯定会恢复官方 recovery 的，关于如何防止恢复官方 recovery 办法：
    
    进入 Recovery 后，如果不是古董机一般都需要做一下防覆盖，否则重启进系统后刷的 recovery 会被官方 recovery 覆盖。
    
    最简单的办法是在卡刷完 官方ROM包 后刷入 `Magisk.zip` (面具)，面具会自动防止覆盖。
    
    如果你并不想 ROOT 但是又不想恢复 官方recovery，可以在卡刷完 ROM 包后签名 boot.OrangeFox 的防止还原 Recovery 的功能，在 `设置`–> `OTA`下。
    
    有的 TWRP 的高级设置中并没有签名boot功能，但是一般情况下都会有防止覆盖TWRP功能，虽然功能实现原理不一样，但是都可以防止恢复官方recovery.

!!! help
    **橙狐设置简体中文界面**
    
    点击底部的 `Menu` –> 右上角的齿轮图标 –> `Regional` –> 点击 `Language` 下的 `English` ，选择 `Chinese Simplified` 。

[^13]


### Magisk侧载[^44]

先下载 Magisk 到你的电脑设备，接着重启至你所使用的第三方 Recovery

```
adb reboot recovery
```

选择 `Apply update Apply from ADB` 以启动 ADB Sideload 模式，刷入 Magisk

``
adb sideload Magisk-xxx.apk
``
>将 Magisk-xxx.apk 替换为你所下载的文件名

完成后，在 Recovery 中重启手机至系统.

最后，安装 Magisk Manager.
```
adb install Magisk-xxx.apk
```

打开手机上新增的 Magisk 应用，若下方的 超级用户 和 模块 菜单能够点击，就代表已经成功获取了 Root 权限.

### 引录

Redmi Note 7 Pro教程

[如何在 Redmi Note 7 Pro 上刷机？](https://blog.linioi.com/posts/11/)

相关Recovery下载

[小米Recovery下载|Recovery大全-FiimeROM](https://mi.fiime.cn/Recovery)

[Devices](https://twrp.me/Devices/)

[OrangeFox Recovery Downloads](https://orangefox.download/zh-CN)

[偶然发现的仓库](https://kamiui.ml/E52shuaji/)



