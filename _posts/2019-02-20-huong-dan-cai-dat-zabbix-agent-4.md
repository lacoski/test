---
date: 2019-02-20
title: Hướng dẫn cài đặt zabbix agent 4.0
categories:
  - Monitor
description: Hướng dẫn cài đặt zabbix agent lên host cần monitor chạy trên các hệ điều hành Centos, Ubuntu, Linux.
author: duydm
tags: [Zabbix]
type: Document
---

Sau khi triển khai <a href="https://blog.cloud365.vn/monitor/cai-dat-zabbix-4-lts-tren-centos7/" target="_blank">zabbix server</a> để có thể giám sát được các host client khác phải cài đặt `zabbix-agent` lên client, tùy theo cơ chế lựa chọn (active check, passive check, SSH check...) cho việc nhận metric từ các host. Ở bài hướng dẫn này giới thiệu cho bạn các bước để cài đặt `zabbix-agent 4.0` lên host client trên hệ điều hành khác nhau (Windows, <a href="https://blog.cloud365.vn/linux/huong-dan-cai-dat-centos7/" target="_blank">CentOS server</a>, Ubuntu).

### Mục lục

[1. Mô hình triển khai](#mohinh)<br>
[2. IP Planning](#planning)<br>
[3. Zabbix-agent CentOS](#centos)<br>
[4. Zabbix-agent Ubuntu](#ubuntu)<br>
[5. Zabbix-agent Windows](#windows)<br>

<a name="mohinh"></a>
## 1. Mô hình triển khai

![](/images/img-agent-zabbix/topo-agent.png)

<a name="planning"></a>
## 2. IP Planning

![](/images/img-agent-zabbix/Screenshot_978.png)

<a name="centos"></a>
## 3. Zabbix-agent CentOS

+ Cài đặt zabbix-agent

**Truy cập host CentOS `10.10.10.119`**

![](/images/img-agent-zabbix/Screenshot_1001.png)

**Thêm zabbix agent repository**

CentOS 7 / RHEL 7

```
rpm -ivh https://repo.zabbix.com/zabbix/4.0/rhel/7/x86_64/zabbix-release-4.0-1.el7.noarch.rpm
```

CentOS 6 / RHEL 6

```
rpm -ivh https://repo.zabbix.com/zabbix/4.0/rhel/6/x86_64/zabbix-release-4.0-1.el6.noarch.rpm
```

**Cài đặt**

```
yum install -y zabbix-agent
```

**Check zabbix-agent**

```
zabbix_agentd -V
```
![](/images/img-agent-zabbix/Screenshot_981.png)

+ Cấu hình zabbix-agent

```
cp /etc/zabbix/zabbix_agentd.conf /etc/zabbix/zabbix_agentd.conf.bk
sed -i 's/Server=127.0.0.1/Server=10.10.10.115/g' /etc/zabbix/zabbix_agentd.conf
sed -i 's/# ListenPort=10050/ListenPort=10050/g' /etc/zabbix/zabbix_agentd.conf
sed -i 's/ServerActive=127.0.0.1/ServerActive=10.10.10.115/g' /etc/zabbix/zabbix_agentd.conf
systemctl enable zabbix-agent
systemctl start zabbix-agent
systemctl restart zabbix-agent
systemctl status zabbix-agent
```

![](/images/img-agent-zabbix/Screenshot_982.png)

Như vậy `zabbix client` CentOS đã sẵn sàng gửi metric về `zabbix server`.

<a name="ubuntu"></a>
## 4. Zabbix-agent Ubuntu

**Truy cập host CentOS `10.10.10.105`**

![](/images/img-agent-zabbix/Screenshot_983.png)

**Thêm zabbix agent repository**

Ubuntu 18.04

```
wget https://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.0-2+bionic_all.deb
dpkg -i zabbix-release_4.0-2+bionic_all.deb
apt-get update
```

Ubuntu 16.04

```
wget https://repo.zabbix.com/zabbix/4.0/ubuntu/pool/main/z/zabbix-release/zabbix-release_4.0-2+xenial_all.deb
dpkg -i zabbix-release_4.0-2+xenial_all.deb
apt-get update
```

**Cài đặt**

```
apt-get install -y zabbix-agent
```

**Check zabbix-agent**

```
zabbix_agentd -V
```
![](/images/img-agent-zabbix/Screenshot_984.png)

+ Cấu hình zabbix-agent

```
cp /etc/zabbix/zabbix_agentd.conf /etc/zabbix/zabbix_agentd.conf.bk
sed -i 's/Server=127.0.0.1/Server=10.10.10.115/g' /etc/zabbix/zabbix_agentd.conf
sed -i 's/# ListenPort=10050/ListenPort=10050/g' /etc/zabbix/zabbix_agentd.conf
sed -i 's/ServerActive=127.0.0.1/ServerActive=10.10.10.115/g' /etc/zabbix/zabbix_agentd.conf
systemctl enable zabbix-agent
systemctl start zabbix-agent
systemctl restart zabbix-agent
systemctl status zabbix-agent
```

![](/images/img-agent-zabbix/Screenshot_985.png)

<a name="windows"></a>
## 5. Zabbix-agent Windows

+ Cài đặt

**Truy cập host Windows `10.10.10.109`**

![](/images/img-agent-zabbix/Screenshot_990.png)

**Download zabbix agent Windows**

Truy cập đường link <a href="http://www.suiviperf.com/zabbix/old/" target="_blank">tại đây</a>.

Lưu ý lựa chọn phiên bản phù hợp với hệ điều hành để tải về.

![](/images/img-agent-zabbix/Screenshot_988.png)


**Cài đặt**

![](/images/img-agent-zabbix/Screenshot_991.png)
![](/images/img-agent-zabbix/Screenshot_992.png)

Đồng ý với điều khoản nhà cung cấp

![](/images/img-agent-zabbix/Screenshot_993.png)

Nhập thông tin zabbix server muốn kết nối tới.

![](/images/img-agent-zabbix/Screenshot_994.png)

Quá trình cài đặt

![](/images/img-agent-zabbix/Screenshot_995.png)
![](/images/img-agent-zabbix/Screenshot_996.png)
![](/images/img-agent-zabbix/Screenshot_997.png)

Kết thúc cài đặt

![](/images/img-agent-zabbix/Screenshot_998.png)

+ Kiểm tra và thay đổi cấu hình

Mở cửa sổ `cmd` -> Nhập `services.msc`

![](/images/img-agent-zabbix/Screenshot_999.png)

Khi bạn muốn thay đổi cấu hình `zabbix-agent` thay đổi file `zabbix_agentd.conf` ở thư mục `C:\Program Files\Zabbix Agent`

![](/images/img-agent-zabbix/Screenshot_1000.png)

Hy vọng những hướng dẫn trên giúp bạn có thể cài đặt thành công zabbix-agent.

---
Thực hiện bởi <a href="https://cloud365.vn/" target="_blank">cloud365.vn</a>