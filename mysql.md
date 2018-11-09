# Redis 설치

```sh
yum -y install redis
systemctl enable redis
systemctl restart redis
```

# MySQL/Maria DB 설치

## MySQL 5.7
- https://www.lesstif.com/pages/viewpage.action?pageId=24445108#RHEL/CentOS,Ubuntu%EC%97%90MySQL5.6,5.7%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-RHEL/CentOS7

1. repository 설치

  ```sh
  rpm -ivh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
  ```
  
1. MySQL 설치

 ```sh
  yum install mysql-community
  ```

1. systemctl

   ```sh
   systemctl enablemysqld
   systemctl start mysqld
   ```

1123

# 설정

