# linux_operation

## 1、查看wifi 密码

```
1、$ cd /etc/NetworkManager/system-connections/
2、$ ls
 Gee  Jane  PDCN
3、$ sudo cat Jane
[connection]
id=Jane
uuid=89755199-7690-4e8d-9405-9c63ecaada63
type=wifi
permissions=
secondaries=

[wifi]
hidden=true
mac-address=40:A3:CC:0C:89:E7
mac-address-blacklist=
mac-address-randomization=0
seen-bssids=
ssid=Jane

[wifi-security]
group=
key-mgmt=wpa-psk
pairwise=
proto=
psk=12345678       --------password

[ipv4]
dns-search=
method=auto

[ipv6]
addr-gen-mode=stable-privacy
dns-search=
method=auto

```

