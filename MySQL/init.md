* 리눅스에서 MySQL 설치

```
$ sudo apt-get update
$ sudo apt-get install -y mysql-server-5.7
$ mysql_secure_installation
```
> apt-get install -y 옵션을 넣으면 설치 중 패키지 설치의 동의를 묻는 과정이 생략된다. 

* MySQL prompt 실행 및 종료

```
$ service mysql start
$ mysql -u root -p
$ exit
$ service my sql stop
```

* root 계정 비밀번호 번경 및 user 확인 
```
mysql > ALTER USER 'root'@'localhost IDENTIFIED WITH mysql_native_password BY 'password'
mysql > FLUSH PRIVILEGES;
mysql > SELECT user from mysql.user
```
> FLUSH PRIVILEGES : GRANT 테이블을 reload하여 변경 사항을 즉시 반영시킨다. 