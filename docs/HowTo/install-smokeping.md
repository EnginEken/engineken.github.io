### How to Install SmokePing on CentOS

- Aşağıdaki şekilde önce network tanımı yapılır ve değişikliklerin aktif olması için network servisi restart edilir.

<div class="termy">

```console
$ vi /etc/sysconfig/network-scripts/ifcfg-ens192
$ systemctl restart network
```
</div>

!!! info inline center "Asya Örnek Network Tanımı"
	```console
	TYPE="Ethernet"
	PROXY_METHOD="none"
	BROWSER_ONLY="no"
	BOOTPROTO="none"
	DEFROUTE="yes"
	IPV4_FAILURE_FATAL="no"
	IPV6INIT="no"
	IPV6_AUTOCONF="yes"
	IPV6_DEFROUTE="yes"
	IPV6_FAILURE_FATAL="no"
	IPV6_ADDR_GEN_MODE="stable-privacy"
	NAME="ens192"
	DEVICE="ens192"
	ONBOOT="yes"
	IPADDR="10.170.250.100"
	PREFIX="24"
	GATEWAY="10.170.250.1"
	DNS1="10.170.166.40"
	DNS2="10.170.166.41"
	```

!!! info inline "Avrupa Örnek Network Tanımı"
	```console
	TYPE="Ethernet"
	PROXY_METHOD="none"
	BROWSER_ONLY="no"
	BOOTPROTO="none"
	DEFROUTE="yes"
	IPV4_FAILURE_FATAL="no"
	IPV6INIT="no"
	IPV6_AUTOCONF="yes"
	IPV6_DEFROUTE="yes"
	IPV6_FAILURE_FATAL="no"
	IPV6_ADDR_GEN_MODE="stable-privacy"
	NAME="ens192"
	DEVICE="ens192"
	ONBOOT="yes"
	IPADDR="10.168.250.100"
	PREFIX="24"
	GATEWAY="10.168.250.1"
	DNS1="192.168.166.40"
	DNS2="192.168.166.41"
	```

- Hostname tanımı yapılır
hostnamectl set-hostname Smokeping-



--Smokeping Kurulum Adımları
yum -y install epel-release
yum -y install smokeping
yum -y update

--Makine reboot edilir
reboot


--Web Arayüzü için Apache Tanımı
vim /etc/httpd/conf.d/smokeping.conf dosyası içerisindeki aşağıdaki local tanımı all granted olarak editlenir
Require local >> Require all granted



--Serviceler restart edilir.
systemctl enable smokeping
systemctl restart smokeping
systemctl restart httpd


--http://<server_ip>/smokeping/sm.cgi adresi üzerinden arayüze erişilir.