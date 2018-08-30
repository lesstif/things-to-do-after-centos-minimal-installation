주요 package 설치

# Manual page

```sh
yum -y install man man-pages kernel-doc
```

# network utility

nslookup, 텍스트 브라우저

```sh
yum -y install bind-utils wget elinks
```

# system utility

```sh
yum -y install yum-utils sysstat lsof 
```

firewalld 에 익숙하지 않을 경우 text 용 설정 툴인 firewall-tui 설치

```sh
yum -y install  system-config-firewall-tui
```

# git & etc

```sh
yum -y install vim git zsh bash-completion unzip bzip2 xz
```

# SELinux utility

SELinux 를 사용할 경우 *audit2why*, *audit2allow*, *seinfo*, *sesearch* 유틸리티 설치

```sh
yum -y install policycoreutils-python setools-console
```

SE trouble shooting server 를 사용할 경우 아래 패키지 설치

```sh
yum -y install setroubleshoot setroubleshoot-server setroubleshoot-doc setroubleshoot-plugins
```
