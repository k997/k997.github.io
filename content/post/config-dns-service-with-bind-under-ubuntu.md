---
title: "Ubuntu 下 Bind 配置 Dns 域名服务"
date: 2022-09-24T10:46:59Z
draft: false
tags: ["网络","DNS","bind"]
---

## 背景

服务器上 `systemd-resolved` 经常挂掉，导致无法解析域名，排查问题无果遂决定使用 bind 替代 `systemd-resolved` 。

## 安装 bind

```
sudo apt update
sudo apt install bind9
```

## 编辑配置文件

编辑 bind 配置文件 `/etc/bind/named.conf.options` , 配置上游 dns 服务器 `forwarders { 8.8.8.8;};` 及监听地址 `listen-on { any; };` ,最终配置文件如下：

```json
options {
        directory "/var/cache/bind";

        // If there is a firewall between you and nameservers you want
        // to talk to, you may need to fix the firewall to allow multiple
        // ports to talk.  See http://www.kb.cert.org/vuls/id/800113

        // If your ISP provided one or more IP addresses for stable
        // nameservers, you probably want to use them as forwarders.
        // Uncomment the following block, and insert the addresses replacing
        // the all-0's placeholder.

        forwarders {
            8.8.8.8;
        };

        //========================================================================
        // If BIND logs error messages about the root key being expired,
        // you will need to update your keys.  See https://www.isc.org/bind-keys
        //========================================================================
        dnssec-validation auto;

        auth-nxdomain no;    # conform to RFC1035
        listen-on { any; };
        listen-on-v6 { any; };
};
```

## 启用 bind 服务

```bash
# 禁用 systemd-resolved
sudo systemctl disable systemd-resolved.service
# 启用 bind
sudo systemctl enable bind9 --now
```
