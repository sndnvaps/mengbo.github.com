---
layout: post
title: "ESXi安装M81KR网卡的驱动过程"
description: ""
category: tech
tags: [vmware, esxi, cisco, ucs, m81kr, driver]
---
{% include JB/setup %}

配置CISCO UCS时候，CISCO的人提示我们M81KR卡的驱动有[更新](https://my.vmware.com/web/vmware/details/dt_esx50_cisco_enic_21222/dHRAYnRAanBiZHAlZA==)，建议我们升级驱动。

由于我们的vShpere系统已经部署完毕，安装VIB包的时候一直提示一个关于FDM的错误，现在已经不记得具体内容了。经过一番google，发现[KB: 2006034](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=2006034&sliceId=1&docTypeID=DT_KB_1_1&dialogID=294834206&stateId=1%200%20297722481)提到的情况和我们遇到的现象比较类似，于是就下决心删除FDM，然后安装驱动。

下面ESXi上执行的脚本可以描述问题：

{% highlight bash %}
esxcli software vib list |grep vmware-fdm
cp /opt/vmware/uninstallers/VMware-fdm-uninstall.sh /tmp
/tmp/VMware-fdm-uninstall.sh
esxcli software vib list |grep vmware-fdm

VIBURL=http://http_server_ip:port/net-enic-2.1.2.22-1OEM.500.0.0.441683.x86_64.vib
esxcli software vib list |grep net-enic
esxcli software vib update -v $VIBURL
{% endhighlight %}

执行完脚本后，需要重启ESXi主机，并且需要重新为该主机配置HA。
