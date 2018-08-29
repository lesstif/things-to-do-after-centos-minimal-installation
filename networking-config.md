# Networking config

네트워크 환경 설정

## Static IP 설정

```sh
vi /etc/sysconfig/network-scripts/ifcfg-*
```


```sh
TYPE=Ethernet
PROXY_METHOD=none
BROWSER_ONLY=no
BOOTPROTO=static
DEFROUTE=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_FAILURE_FATAL=no
IPV6_ADDR_GEN_MODE=stable-privacy
NAME=enp5s0f0
DEVICE=enp5s0f0
ONBOOT=yes
IPADDR="192.168.0.100"
NETMASK="255.255.255.224"
GATEWAY="192.168.0.1"
DNS1="168.126.63.1"
DNS2="8.8.8.8"
#ZONE=dmz
```

### 필수 입력

다음 항목에 적절한  Network 정보를 기술

* BOOTPROTO: 고덩 IP 를 사용하므로 static 으로 설정
* IPADDR : 
* NETMASK :
* GATEWAY :
* DNS1: first DNS
* DNS2: second DNS

ZONE 은 Firewalld 에 적용할 Network Zone 이름 설정

> **Warning** *ONBOOT=yes* 로 설정해야 부팅시 자동으로 네트워크를 설정합니다.

## systemd 구동

다음 명령어로 network 설정을 반영합니다.

```sh
systemctl restart network
```

## DNS 설정 여부 확인

DNS 가 정상 설정되었다면 */etc/resolv.conf* 에 설정이 반영됩니다.

아래 명령어로 정상 설정 여부를 확인할 수 있습니다.

```sh
$ cat /etc/resolv.conf

nameserver 168.126.63.1
nameserver 8.8.8.8
```
## Network 정상 설정 확인

network 을 사용하는 유틸리티를 구동해서 정상 설정 여부를 확인하면 됩니다.

```sh
nslookup google.com
```
> **Info** *nslookup* 은 minimal install 시 설치되지 않으므로 *command not found* 에러가 발생할 수 있습니다.

curl 은 최소 설치시에도 설치됩니다.

```sh
curl -v google.com
```
