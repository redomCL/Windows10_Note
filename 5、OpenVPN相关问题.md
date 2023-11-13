### https://me.jinchuang.org/archives/573.html

### 开机自动启动OpenVPN后自动链接：
* 创建快捷方式，在后面加入" --connect client.ovpn"

### WARNING: this configuration may cache passwords in memory -- use the auth-nocache option to prevent this：
* auth-nocache

### DEPRECATED OPTION: --cipher set to 'AES-256-CBC' but missing in --data-ciphers (AES-256-GCM:AES-128-GCM). OpenVPN ignores --cipher for cipher negotiations.：
* data-ciphers-fallback AES-256-CBC

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### Windows10客户端使用OpenVPN软件连接过程中可能会遇到几个红色警告或错误信息，我也是在使用中有遇到这些问题，网上搜索的方法可以解决掉遇到的问题(不保证所有遇到此问题的都可以通过下面方法解决)，特此搜集记录下来在连接vpn有问题情况下，确认服务和端口是否正常和允许连接，多观察服务端日志和客户端日志的异常输出信息，大概率是客户端参数问题或者配置不正确，或者是网络方面的问题。

* 1 客户端版本也会造成一些警告提示，因为会有些参数在某个版本中不支持了
* 2 遇到多百度谷歌搜索相关信息，多动手改调整配置参数
* 3 确保配置文件的参数名正确，文件路径或格式正确
* 4 有时候或许是网络问题导致连不上，可以换换其他网络试试

警告1：WARNING: --ns-cert-type is DEPRECATED. Use --remote-cert-tls instead.或者No sever certificate verficaton method has been enabled.

* 解决，wWindows客户端修改配置文件注释一个和添加一个（如果没有ns-cert-type server这个参数，那只用添加remote-cert-tls server这个参数）
* ns-cert-type server
* remote-cert-tls server

警告2： WARNING: this configuration may cache passwords in memory -- use the auth-nocache option to prevent this

* 解决，在客户端配置文件添加参数
* auth-nocache

警告3：WARNING: 'link-mtu' is used inconsistently, local='link-mtu 1602', remote='link-mtu 1570'WARNING: 'tun-mtu' is used inconsistently, local='tun-mtu 1532', remote='tun-mtu 1500'

* 解决
* 去掉配置文件tun-mtu相关的参数

警告4：(此错误会导致连上vpn无法上网)： us=967576 WARNING: 'comp-lzo' is present in remote config but missing in local config, remote='comp-lzo'或 write to TUN/TAP : Unknown error (code=122)

* 解决，在客户端配置文件添加参数
* comp-lzo

警告5： ROUTE: route addition failed using CreateIpForwardEntry: ÖÁÉÙÓÐһ¸ֲÎÊ�Õýȷc [status=160 if_index=19]

* 解决，在客户端配置文件添加参数
* route-method exe
* route-delay 2

警告6：AEAD Decrypt error: bad packet ID (may be a replay): [ #141 ] -- see the man page entry for --no-replay and --replay-window for more info or silence this warning with --mute-replay-warnings

* 解决，在客户端配置文件添加参数
* mute-replay-warnings

警告7: WARNING: --ping should normally be used with --ping-restart or --ping-exit

* 解决，配置文件中参数修改为下面的
* ping 表示每隔多少秒就ping对端一次
* ping-restart/ping-exit 表示在多少秒后没有收到对端的包就重启或者退出
* ping 15
* ping-restart 15 或者 ping-exit 15

警告8（和客户端版本有关系）: DEPRECATED OPTION: --cipher set to 'AES-256-CBC' but missing in --data-ciphers (AES-256-GCM:AES-128-GCM). OpenVPN ignores --cipher for cipher negotiations.

* 解决,配置文件中参数修改为下面的
* data-ciphers AES-256-CBC
* data-ciphers-fallback AES-256-CBC

警告9（和客户端版本有关系）: WARNING: Compression for receiving enabled. Compression has been used in the past to break encryption. Sent packets are not compressed unless "allow-compression yes" is also set.

* 解决,不启用压缩，配置文件中参数修改为下面的
* comp-lzo no
* allow-compression no

错误1: TLS Error: TLS key negotiation failed to occur within 60 seconds (check your network connectivity) TLS Error: TLS handshake failed

* 可能的原因1（来自官网的回答 https://openvpn.net/faq/tls-error-tls-key-negotiation-failed-to-occur-within-60-seconds-check-your-network-connectivity/）
* 1，服务器网络上的外围防火墙正在过滤传入的 OpenVPN 数据包（默认情况下，OpenVPN 使用 UDP 或 TCP 端口号 1194）。在 OpenVPN 服务器机器上运行的软件防火墙本身正在过滤端口 1194 上的传入连接。请注意，默认情况下许多操作系统将阻止传入连接，除非另有配置。
* 2，服务器网络上的 NAT 网关没有 TCP/UDP 1194 端口转发规则到 OpenVPN 服务器机器的内部地址。
* 3，OpenVPN 客户端配置在其配置文件中没有正确的服务器地址。客户端配置文件中的remote指令  必须指向服务器本身或服务器网络网关的公共 IP 地址。
* 4，另一个可能的原因是 Windows 防火墙阻止了对 openvpn.exe 二进制文件的访问。您可能需要将其列入白名单（将其添加到“例外”列表），OpenVPN 才能正常工作。

* 可能的原因2
* 配置文件的问题

* 特别说明：如果客户端连接不上，报错信息没有给出明显的错误，请在服务端的日志中查看日志，会看到是什么原因导致的！如果同样配置文件已经在其他电脑连接成功，请对比下配置文件内容，有用户密码的核对下和服务端的是否一致或者两个人用一套证书都连接上了但是有1个人不能上网的，可能是服务器没有开启同一套证书允许多用户使用

客户端连接不上几个提示例子:

提示1：
MANAGEMENT: >STATE:1679479405,TCP_CONNECT,,,,,,(一直卡这很久)
TCP: connect to [AF_INET]x.x.x.x:1194 failed: Unknown error

提示2：
MANAGEMENT: >STATE:1679479100,WAIT,,,,,,
read UDP: Unknown error (code=10054)

提示3：
UDP link remote: [AF_INET]x.x.x.x:1194
MANAGEMENT: >STATE:1679485965,WAIT,,,,,,

* 以上几种提示是连不上才出现的（客户端连不到服务端） [ ip地址错误 或 使用协议错误 或 服务端拒绝连接 ]

* 客户端的原因导致：
* 可能是1、客户端和服务端配置的协议不一致
* 可能是2、端口 或 地址 不正确

* 服务端的原因导致：
* 可能是3、服务端服务未启动
* 可能是4、防火墙阻止了对应的端口和协议
* 可能是5、地址被运营商封掉，墙掉了

* 其他原因导致：
* 1、客户端参数和服务端参数不一致
* 2、你的网络运营商给openvpn端口或者协议给封掉了

* pptp、l2tp、openvpn 都属于常见类型vpn，如果是服务端是国外的地址，使用中突然连不上了也很正常！

* 客户端和服务端已经建立连接，但是连接过程中协商出错，也会有waring或error的提示，这种都是配置文件中的参数配置的有些问题，或者是缺少了证书文件导致的（和服务端建立连接，建立连接过程中出现 身份信息错误 或 缺少必要的信息）
