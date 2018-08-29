# Apache HTTPD

```sh
yum -y install httpd mod_ssl
```

# Nginx

## nginx 저장소 설정

root 권한으로 아래 스크립트 실행(shell 에 복사 & 붙여넣기)

```sh
cat << EOF > /etc/yum.repos.d/nginx.repo

[nginx]
name=Nginx Repository \$basearch - Archive
baseurl=http://nginx.org/packages/centos/\$releasever/\$basearch/
enabled=1
gpgcheck=1
gpgkey=https://nginx.org/keys/nginx_signing.key
EOF
```

## 설치

```sh
yum -y install nginx
```

부팅시 자동 구동되도록 설정

```sh
systemctl enable nginx
systemctl restart nginx
```


## 가상 호스트 설정

우분투는 가상 호스트 설정은 sites-available 디렉터리에 가상 호스트별 설정 파일(lesstif.com 처럼 호스트 이름으로 하는게 제일 깔끔한 것 같다)을 위치시키고
활성화할 가상 호스트는 sites-enabled/ 디렉터리에 위치하도록 하고 있다. 

sites-enabled/ 에 있는 가상 호스트 설정은 실제로는  sites-available 에 있는 파일에 대한 심볼릭 링크이다.

```
include /etc/nginx/sites-enabled/*;
```

가상 호스트 설정을 도와주는 utility(https://gist.github.com/lesstif/82c107282241c7a52ad9)

```sh
curl -o serve-php.sh https://gist.githubusercontent.com/lesstif/82c107282241c7a52ad9/raw && sudo mv serve-php.sh /usr/local/bin/ && sudo chmod +x /usr/local/bin/serve-php.sh
```

실행은 다음처럼 도메인명과 webroot 경로를 주면 됨

```sh
sudo serve-php.sh my-new-site /var/www/html/my-new-site-webroot
```

# 같이 보기

* [RHEL/CentOS Ubuntu 에 nginx 설치](https://www.lesstif.com/pages/viewpage.action?pageId=25100304)
* [웹 서버 설정 - 견고한 웹 서비스 만들기](https://lesstif.gitbook.io/web-service-hardening/web-server)
* [HTTPS 설정 - 견고한 웹 서비스 만들기](https://lesstif.gitbook.io/web-service-hardening/ssl-tls-https)
