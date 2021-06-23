# 데이터베이스 생성 및 권한 부여

1. `mysql -u user -p`
2. `CREATE DATABASE DB_NAME default CHARACTER SET UTF8;`
3. `use mysql`
4. `select host, user, password from user;`
5. `GRANT ALL PRIVILEGES ON DB_NAME.* TO 'USER_NAME'@'%';`
  - `GRANT ALL PRIVILEGES ON DB_NAME.* TO 'USER_NAME'@'localhost';`
6. `FLUSH PRIVILEGES;`
