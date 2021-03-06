---
layout: post
title: "nic bonding / teaming in centos 7"
date: 2015-12-22
---

<h3>list all available NICs</h3>

{% highlight language %}
# nmcli -p dev status
================================================
               Status of devices
================================================
DEVICE  TYPE      STATE      CONNECTION
------------------------------------------------
ens192  ethernet  connected  ens192
ens224  ethernet  connected  ens224
lo      loopback  unmanaged  --
[root@centos7 ~]#
{% endhighlight %}

<h3>add interface with ipv4 only address, two slaves and active-backup</h3>
{% highlight language %}
#nmcli connection add type bond ifname bond0 con-name bond0 mode balance-rr primary ens192 miimon 200 ip4 192.168.0.2 gw4 192.168.0.1
#nmcli connection mod bond0 ipv4.dns “192.168.0.1” ipv6.method “ignore”
#nmcli connection add type bond-slave ifname ens192 master bond0
#nmcli connection add type bond-slave ifname ens224 master bond0
{% endhighlight %}

<h3>activate the newly created bond /team connection</h3>
{% highlight language %}
#nmcli c u bond0
Connection successfully activated (D-Bus active path: /org/freedesktop/NetworkManager/ActiveConnection/3)
{% endhighlight %}

<h3>Verify the connectivity.</h3>
{% highlight language %}
#nmcli network connectivity
{% endhighlight %}
<h3>full information about bond connections</h3>
{% highlight language %}
[root@centos7 ~]# nmcli -p connection show bond0
===============================================================================
                      Connection profile details (bond0)
===============================================================================
connection.id:                          bond0
connection.uuid:                        XXXXXXX-XXXXX-XXXX-XXXXX-XXXXXXXX
connection.interface-name:              bond0
connection.type:                        bond
connection.autoconnect:                 yes
connection.autoconnect-priority:        0
<--------------------->
{% endhighlight %}
