# 외부 repository 설치

EPEL 과 WebTatic 저장소 설치

```sh
yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm 
yum -y install https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
```

# PHP 설치

**Info** Laravel 구동하는데 필요한 모듈 위주

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
[PHP]
max_execution_time = 60
memory_limit = 1024M
post_max_size = 128M
upload_max_filesize = 128M
max_file_uploads = 20

[Date]
date.timezone = Asia/Seoul

[curl]
; curl.cainfo =  /etc/ssl/certs/cacert.pem
```

**Info** curl 을 많이 사용할 경우 curl.cainfo 에 다운받은 CA 인증서 묶음(cacert.pem) 파일의 절대 경로를 설정

```sh
curl -k -L -O https://curl.haxx.se/ca/cacert.pem && mv cacert.pem /etc/ssl/certs
```

### 설정 확인

```sh
php -r "phpinfo();"|grep -E "memory_limit|execution_time|post_max_size|upload_max_filesize"
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

# PHP tool 설치

## Composer

```sh
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin/
sudo ln -s /usr/local/bin/composer.phar /usr/local/bin/composer
```

## PHPUnit

### PHPUnit 5.7


```sh
wget --no-check-certificate https://phar.phpunit.de/phpunit-5.7.27.phar
chmod +x phpunit-5.7.*.phar
sudo mv phpunit-5.7.*.phar /usr/local/bin/
 
sudo ln -s /usr/local/bin/phpunit-5.7.*.phar /usr/local/bin/phpunit
sudo ln -s /usr/local/bin/phpunit-5.7.*.phar /usr/local/bin/phpunit5
```

### PHPUnit 6.5

```sh
wget --no-check-certificate https://phar.phpunit.de/phpunit-6.5.9.phar
chmod +x phpunit-6.*.phar
sudo mv phpunit-6.*.phar /usr/local/bin/
 
sudo ln -s /usr/local/bin/phpunit-6.*.phar /usr/local/bin/phpunit6
```

# nginx 가상 호스트 추가 script

## serve-php

*설치:* 

```sh
curl -o serve-php.sh https://gist.githubusercontent.com/lesstif/82c107282241c7a52ad9/raw 
sudo mv serve-php.sh /usr/local/bin/
sudo chmod +x /usr/local/bin/serve-php.sh 
```

*사용:* 

```sh
serve-php.sh myhost.com /var/www/myhost.com
```

{% gist id="lesstif/82c107282241c7a52ad9" %}{% endgist %}

# 같이 보기

* [RHEL/CentOS 5,6,7 에 EPEL 과 Remi/WebTatic Repository 설치하기](https://www.lesstif.com/pages/viewpage.action?pageId=6979743)
* [curl 에 신뢰하는 인증기관 인증서 추가하기](https://www.lesstif.com/pages/viewpage.action?pageId=15892500)
