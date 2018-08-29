# 외부 repository 설치

EPEL 과 WebTatic 저장소 설치

```sh
yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm 
yum -y install https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```

# PHP 설치

**Info* Laravel 구동하는데 필요한 모듈 위주

## PHP 7.1

```sh
yum -y install php71w-cli php71w-common php71w-fpm  php71w-intl \
php71w-gd php71w-mbstring  php71w-mysqlnd php71w-opcache php71w-pdo \
php71w-xml
```

## PHP 7.2

```sh
yum -y install php72w-cli php72w-common php72w-fpm  php72w-intl \
        php72w-gd php72w-mbstring  php72w-mysqlnd php72w-opcache php72w-pdo \
        php72w-sodium php72w-xml
```

# 설정

## php.ini 수정

```

```

## php-fpm 설정

설정 파일(*/etc/php-fpm.d/www.conf*) 수정

```sh
vi /etc/php-fpm.d/www.conf 
```

fpm 설정

```sh
[www]
# 구동 계정 수정
user = lesstif
group = lesstif

pm.max_children = 50

pm.start_servers = 10

pm.min_spare_servers = 10

pm.max_spare_servers = 35

; Pass environment variables like LD_LIBRARY_PATH. All $VARIABLEs are taken from
; the current environment.
; Default Value: clean env
;env[HOSTNAME] = $HOSTNAME
;env[PATH] = /usr/local/bin:/usr/bin:/bin
```

## 서비스 설정

시작 및 부팅시 자동 구동 설정

```sh
systemctl enable php-fpm
systemctl restart php-fpm
```


# Composer install

# 같이 보기

* [RHEL/CentOS 5,6,7 에 EPEL 과 Remi/WebTatic Repository 설치하기](https://www.lesstif.com/pages/viewpage.action?pageId=6979743)
