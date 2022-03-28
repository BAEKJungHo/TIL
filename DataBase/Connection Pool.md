# Connection Pool

요청 마다 커넥션을 생성하는 비용은 너무 크다. 따라서, Thread Pool 처럼 미리 커넥션을 생성해 두고, 요청이 들어오면 놀고 있는 커넥션을 할당해서 사용하고, 사용이 끝나면 반납하는 형식이다.

## MySQL Connection Pool

MS SQL(SQL Server)나 Oracle과 같은 유료 DBMS는 타임아웃이란 개념이 존재한다.
단순하게 말하자면 이러한 타임아웃은 Connection 을 자동으로 `release(접속종료)` 해주는 것을 의미하는데, 무료 DBMS 인 MySQL 은 기본적으로 이러한 Connection 을 
자동으로 release 해주지 않는다. 하지만 MySQL 에서 Connection Pool 을 사용하면 다음과 같은 형태로 Auto Release 를 지원한다. 

`pool.query() = pool.getConnection() + connection.query() + connection.release()`

### Node.js 에서 Connection Pool 구현

```
const mysql = require('mysql2');
const pool = mysql.createPool({ 
  host: 'localhost',
  user: 'root', 
  database: 'test', 
  waitForConnections: true,
  connectionLimit: 10, 
  queueLimit: 0 
}); 

pool.query("SELECT field FROM atable", function(err, rows, fields) { 
  // Connection is automatically released when query resolves 
}) 

pool.getConnection(function(err, conn) { 
  // Do something with the connection 
  conn.query(/* ... */);
  // Don't forget to release the connection when finished! 
  pool.releaseConnection(conn); 
})
```


## References

- https://cotak.tistory.com/105
- https://github.com/mysqljs/mysql/issues/1202
