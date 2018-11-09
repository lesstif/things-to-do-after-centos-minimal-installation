# Redis 설치

```sh
yum -y install redis
systemctl enable redis
systemctl restart redis
```

# MySQL/Maria DB 설치

## MySQL 5.7
- [MySQL 5.7 설치](https://www.lesstif.com/pages/viewpage.action?pageId=24445108#RHEL/CentOS,Ubuntu%EC%97%90MySQL5.6,5.7%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0-RHEL/CentOS7)


1. repository 설치

  ```sh
  rpm -ivh https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
  ```
  
1. MySQL 설치

 ```sh
  yum install mysql-community-server
  ```

1. 기존 암호 규칙을 사용하기 위해 password 정책 수정

   ```sh
   echo 'validate_password_policy=LOW' >> /etc/my.cnf
   echo 'default_password_lifetime=0' >> /etc/my.cnf
   ```

1. MySQL 초기 암호 확인

   ```sh
   grep 'temporary password' /var/log/mysqld.log
   ```
   
1. MySQL root 로 로그인   
   
   
   ```sh
   mysql -u root -p
   ```

1. MySQL 초기 암호 변경
   
   ```sh
   mysql> set password=password('qwert123');
   mysql> flush privileges;
   ```
   
1. systemctl 로 자동 구동

   ```sh
   systemctl enable mysqld
   systemctl start mysqld
   ```


# 설정


1. my.cnf 설정

시스템별 최적 설정은 https://tools.percona.com/wizard 에서 설정

  ```sh
  vim /etc/my.cnf
  ```
  
   ```
   [mysqld]
   collation-server = utf8mb4_unicode_ci
   character-set-server = utf8mb4
   skip-character-set-client-handshake
   
   default-storage-engine         = InnoDB
   socket                         = /var/lib/mysql/mysql.sock
   pid-file                       = /var/lib/mysql/mysql.pid
   
   max-allowed-packet             = 256M
   datadir                        = /var/lib/mysql/
   
   # # LOGGING #
   log-error                      = /var/lib/mysql/mysql-error.log
   log-queries-not-using-indexes  = 1
   slow-query-log                 = 1
   slow-query-log-file            = /var/lib/mysql/mysql-slow.log
   
   # Disabling symbolic-links is recommended to prevent assorted security risks
   symbolic-links=0
   
   validate_password_policy=LOW
   default_password_lifetime=0
   ```
