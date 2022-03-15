# Privoxy-Shadowsocks On Ubuntu 20.04 TLS

![privoxy-shadowsocks](https://netslovers.com/wp-content/uploads/2022/03/privoxy-shadowsocks-ubuntu-server-proxy.png)


Using Privoxy beside Shadowsocks, as the Shadowsocks Tunneling Proxy Server has its specific URI schema to use at the client-side, which starting will  `ss://`  

But with Privoxy as an interface, we can enjoy Shadowsocks regulare with the outilne clients, and same time connecting it with our browsers and apps as HTTP/HTTPS Proxy using Privoxy.

The Full Article: [Build Privoxy And Shadowsocks Proxy Servers On Ubuntu 20.04](https://netslovers.com/2022/03/15/privoxy-shadowsocks-proxy-servers-ubuntu/) 

We will:

1. Update Ubuntu And Install The Prerequisites
2. Install Shadowsocks Python Server
3. Configure Shadowsocks
4. Install Privoxy
5. Configure Privoxy
6. Start The Servers
7. Tuning Ubuntu for network performance
8. Using Shadowsocks Client
9. Using Privoxy HTTP(S)

## Configure Puplic and Local Shadowsocks server

The Shadowsocks local (sslocal) config file is `/etc/shadowsocks.local.json` [is here](./shadowsocks.local.json)

The Shadowsocks public (ssserver) config file is `/etc/shadowsocks.json` [is here](./shadowsocks.json)

The deffrent between the 2 files, is that we specify "pid-file":"/var/run/shadowsocks.local.pid" for the local Daemon


We can run the Shadowsocks server for localhost to serve Privoxy as
> sslocal -c /etc/shadowsocks.local.json -d start

And run the public server to listen to outside world
> ssserver -c /etc/shadowsocks.json -d start


## Configure Privoxy to forward requests to shadowsocks local server at port 1080
The config file ` /etc/privoxy/config `
~~~
#listen-address 127.0.0.1:8118
#listen-address [::1]:8118
listen-address [Public_IP_Address]:8118
forward-socks5t / 127.0.0.1:1080 .
#
# 4.2. toggle
# ============
~~~

## Installing Shadowsocks Python Server
We need to run the following command `# pip install https://github.com/shadowsocks/shadowsocks/archive/master.zip`

## Tuning Kernel Configs for network performace
Set BBR tcp_congestion_control And Tunning The Kernel For Shadowsocks
Create the file [local.conf](https://github.com/linuxlabz/Shadowsocks/blob/main/local.conf) into the sysctl configuration path `/etc/sysctl.d/local.conf` and insert the following configs and apply changes by runngin `# sysctl -p`





