> Author: JunJianSyu <br />
> Date: 2017.3.13

##  Shadowsocks

> If you want to keep a secret, you must also hide it from yourself.

## Download

* [clients](https://shadowsocks.org/en/download/clients.html) Download the latest application
* [Servers](https://shadowsocks.org/en/download/servers.html) VPS install source code


## Quick start

Python Example:

```bash
// Debian/Ubuntu
apt-get install python-pip
pip install shadowsocks
// ContOS:
yum install python-setuptools && easy_install pip
pip install shadowsocks
```

usage:
Create a config file `/etc/shadowsocks.json` Example:

*shadowsocks.json*
```bash
{
    "server":"my_server_ip",
    "server_port":8388,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"mypassword",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open": false
}
```

To run in the foreground:

```bash
ssserver -c /etc/shadowsocks.json
```

To run in the background:

```bash
ssserver -c /etc/shadowsocks.json -d start
ssserver -c /etc/shadowsocks.json -d stop
```
