# 데이터베이스 생성 및 권한 부여

- `CREATE DATABASE DB_NAME default CHARACTER SET UTF8;`
- `GRANT ALL PRIVILEGES ON DB_NAME.* TO 'USER_NAME'@'%';`
- `GRANT ALL PRIVILEGES ON DB_NAME.* TO 'USER_NAME'@'localhost';`
- `FLUSH PRIVILEGES;`

# USER 조회

- mysql -u user - p
- use mysql
- select host, user, password from user;
