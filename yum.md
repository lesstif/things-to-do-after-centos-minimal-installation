yum repository 설정

# CentOS 국내 미러 사이트 변경

```sh
vi /etc/yum.repos.d/CentOS-Base.repo 
```

***mirrorlist*** 를 주석처리하고 ***baseurl*** 에 국내 미러 사이트를 설정 

```
[base]
name=CentOS-$releasever - Base
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/os/$basearch/
baseurl=http://centos.mirror.cdnetworks.com/$releasever/os/$basearch/
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7

[updates]
name=CentOS-$releasever - Updates
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates&infra=$infra
#baseurl=http://mirror.centos.org/centos/$releasever/updates/$basearch/
baseurl=http://centos.mirror.cdnetworks.com/$releasever/updates/$basearch
gpgcheck=1
gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7
```

# fast mirror plugin 중지

빠른 미러 사이트를 찾아주는 fast mirror plugin 중지

```sh
vi /etc/yum/pluginconf.d/fastestmirror.conf 
```

아래 내용중에 *enabled=1* 을 *enabled=0* 으로 변경

```
[main]
enabled=0
verbose=0
```

# 국내 미러 사이트

## CD Networks

```
[base]
baseurl=http://centos.mirror.cdnetworks.com/$releasever/os/$basearch/

[updates]
baseurl=http://centos.mirror.cdnetworks.com/$releasever/updates/$basearch
```

## Kakao

```
[base]
baseurl=http://mirror.kakao.com/centos/$releasever/os/$basearch/

[updates]
baseurl=http://mirror.kakao.com/centos/$releasever/updates/$basearch
```


## Naver

```
[base]
baseurl=http://mirror.navercorp.com/centos/$releasever/os/$basearch/

[updates]
baseurl=http://mirror.navercorp.com/centos/$releasever/updates/$basearch/
```

# 같이 보기

* [CentOS yum 과 Ubuntu apt Mirror를 국내 사이트로 설정하기](https://www.lesstif.com/pages/viewpage.action?pageId=20776717)
