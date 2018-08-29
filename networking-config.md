# Networking config

네트워크 환경 설정

## Static IP 설정

```sh
vi /etc/sysconfig/network-scripts/ifcfg-*
```

시스템의 network interface 설정 파일을 편집합니다.

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

# Kernel parameter 수정

## sysctl 수정

sysctl 설정 파일인 */etc/sysctl.conf* 수정

```sh
vi /etc/sysctl.conf
```

```
# Protection from SYN flood attack.
net.ipv4.tcp_syncookies = 1

# See evil packets in your logs.
net.ipv4.conf.all.log_martians = 1

# Discourage Linux from swapping idle server processes to disk (default = 60)
vm.swappiness = 10

# Be less aggressive about reclaiming cached directory and inode objects
# in order to improve filesystem performance.
vm.vfs_cache_pressure = 50

# --------------------------------------------------------------------
# The following allow the server to handle lots of connection requests
# --------------------------------------------------------------------

# Increase number of incoming connections that can queue up
# before dropping
net.core.somaxconn = 50000

# Handle SYN floods and large numbers of valid HTTPS connections
net.ipv4.tcp_max_syn_backlog = 30000

# Increase the length of the network device input queue
net.core.netdev_max_backlog = 5000

# Increase system file descriptor limit so we will (probably)
# never run out under lots of concurrent requests.
# (Per-process limit is set in /etc/security/limits.conf)
fs.file-max = 100000

# Widen the port range used for outgoing connections
net.ipv4.ip_local_port_range = 10000 65000

# If your servers talk UDP, also up these limits
net.ipv4.udp_rmem_min = 8192
net.ipv4.udp_wmem_min = 8192

# --------------------------------------------------------------------
# The following help the server efficiently pipe large amounts of data 
# --------------------------------------------------------------------

# Disable source routing and redirects
net.ipv4.conf.all.send_redirects = 0
net.ipv4.conf.all.accept_redirects = 0
net.ipv4.conf.all.accept_source_route = 0

# Disable packet forwarding.
net.ipv4.ip_forward = 0
net.ipv6.conf.all.forwarding = 0

# Disable TCP slow start on idle connections
net.ipv4.tcp_slow_start_after_idle = 0

# Increase Linux autotuning TCP buffer limits
# Set max to 16MB for 1GE and 32M (33554432) or 54M (56623104) for 10GE
# Don't set tcp_mem itself! Let the kernel scale it based on RAM.
# net.core.rmem_max = 16777216
# net.core.wmem_max = 16777216
# net.core.rmem_default = 16777216
# net.core.wmem_default = 16777216
# net.core.optmem_max = 40960
# net.ipv4.tcp_rmem = 4096 87380 16777216
# net.ipv4.tcp_wmem = 4096 65536 16777216


# --------------------------------------------------------------------
# The following allow the server to handle lots of connection churn
# --------------------------------------------------------------------

# Disconnect dead TCP connections after 1 minute
net.ipv4.tcp_keepalive_time = 60

# Wait a maximum of 5 * 2 = 10 seconds in the TIME_WAIT state after a FIN, to handle
# any remaining packets in the network. 
# net.ipv4.netfilter.ip_conntrack_tcp_timeout_time_wait = 5

# Allow a high number of timewait sockets
net.ipv4.tcp_max_tw_buckets = 2000000

# Timeout broken connections faster (amount of time to wait for FIN)
net.ipv4.tcp_fin_timeout = 10

# Let the networking stack reuse TIME_WAIT connections when it thinks it's safe to do so
net.ipv4.tcp_tw_reuse = 1

# Determines the wait time between isAlive interval probes (reduce from 75 sec to 15)
net.ipv4.tcp_keepalive_intvl = 15

# Determines the number of probes before timing out (reduce from 9 sec to 5 sec)
net.ipv4.tcp_keepalive_probes = 5
```

## 사용자 레벨 수정

설정 파일 */etc/security/limits.conf* 수정

```sh
vi /etc/security/limits.conf
```

```
httpd soft nofile 55536
httpd hard nofile 55536

nginx soft nofile 55536
nginx hard nofile 55536

lesstif soft nofile 55536
lesstif hard nofile 55536
```

## 설정 확인

```sh
sysctl -p
```

```sh
ulimit -a
```

# 같이 보기

* [Linux Web Server Kernel Tuning](https://gist.github.com/kgriffs/4027835)
