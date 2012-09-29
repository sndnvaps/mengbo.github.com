---
layout: post
title: "如何对 Android 系统进行 root"
category: tech
tags: [android, root, galaxy nexus, busybox]
---
{% include JB/setup %}

我刚刚换了新的 Android 手机，Galaxy Nexus ，下面的内容都是针对 Galaxy Nexus 的，但对于别的手机来说，原理也是类似的。对于为什么需要刷机，为什么需要 root ，这类问题这里就不详细讨论了；同样，对于如何安装 Android SDK 以及 fastboot 之类的问题这里也不介绍了。

首先是解锁，按照下面的脚本就可以将机器解锁。

{% highlight bash %}
# unlock
adb devices
adb reboot bootloader
sleep 5
fastboot devices
fastboot oem unlock
fastboot reboot
sleep 5
{% endhighlight %}

网络上很多人说解锁后机器的内容会丢失，但在我的机器上内容根本没有丢失，这个可能是 Android 4.1.1 Jelly Bean 的新特性吧，我没有详细考证。

对于 Galaxy Nexus，可以通过如下脚本来刷入 4.1.1 Jelly Bean 的最新固件。

{% highlight bash %}
# flash galaxy nexus
wget https://dl.google.com/dl/android/aosp/yakju-jro03c-factory-3174c1e5.tgz
tar zxvf yakju-jro03c-factory-3174c1e5.tgz
cd yakju-jro03c
adb devices
adb reboot bootloader
sleep 5
./flash-all.sh
sleep 5
{% endhighlight %}

这个脚本中的 flash-all.sh 会将系统的数据彻底清除，建议刷机前做好备份工作。

对于 4.1.1 Jelly Bean 系统，可以通过下面的脚本来开始 root 过程。

{% highlight bash %}
# root galaxy nexus
wget http://downloads.androidsu.com/superuser/Superuser-3.1.3-arm-signed.zip
adb -d shell mkdir -p /sdcard/tmp
adb -d push Superuser-3.1.3-arm-signed.zip /sdcard/tmp
wget http://download2.clockworkmod.com/recoveries/recovery-clockwork-touch-6.0.1
.0-maguro.img
adb reboot bootloader
sleep 5
fastboot boot recovery-clockwork-touch-6.0.1.0-maguro.img
{% endhighlight %}

在执行完脚本后，机器会进入 clockworkmod recovery 的界面，选择安装 Superuser 的 ZIP 文件后重新启动机器，系统就被 root 了。这个方法并没有将系统原有的 recovery 刷成 clockworkmod 的，而仅仅是通过“ fastboot boot ”命令来启动 clockworkmod 的 recovery 镜像，对系统的影响最小。

对于想将机器的用户数据彻底清除，可以通过下面的脚本完成。

{% highlight bash %}
# erase userdata and cache
adb devices
adb reboot bootloader
sleep 5
fastboot -w
fastboot reboot
{% endhighlight %}

按照以上方法刷机并且 root 后，Android 系统就完全在你的控制之中了。对于 root 后的机器，建议首先安装 [BusyBox](https://play.google.com/store/apps/details?id=stericson.busybox&feature=search_result) ，安装时建议选择安装到 /system/xbin 目录中。[adbWireless](https://play.google.com/store/apps/details?id=siir.es.adbWireless&feature=search_result) 和 [SSHDroid](https://play.google.com/store/apps/details?id=berserker.android.apps.sshdroid&feature=search_result) 也是很好的系统软件，建议安装。

对于 root 后的机器，完全可以当作一个简化了的 linux 来使用，可以在系统中安装很多其他类型的软件，例如下面的脚本就可以在系统中安装 curl 这个命令行程序。将这个脚本文件拷贝到机器的 SD 卡中，在机器的命令行下通过执行“ sh 脚本文件名”，就可以完成 curl 的下载和安装任务。

{% highlight bash %}
#!/system/bin/sh
wget -O - http://curl.haxx.se/gknw.net/7.27.0/dist-android/curl-7.27.0-rtmp-ssh2
-ssl-zlib-static-bin-android.tar.gz | tar zxv
su -c "mount -o rw,remount /dev/block/mtdblock3 /system"
su -c "cp curl-7.27.0-rtmp-ssh2-ssl-zlib-static-bin-android/data/local/bin/curl 
/system/xbin"
su -c "chmod 755 /system/xbin/curl"
su -c "mount -o ro,remount /dev/block/mtdblock3 /system"
busybox rm -rf curl-7.27.0-rtmp-ssh2-ssl-zlib-static-bin-android
{% endhighlight %}

