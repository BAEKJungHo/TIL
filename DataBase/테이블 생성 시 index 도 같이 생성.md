# 테이블 생성 시 index 도 같이 생성

```sql
CREATE TABLE `TABLE` (   
  `index` int(11) NOT NULL AUTO_INCREMENT,   
  `date` varchar(10) NOT NULL,   
  `time` varchar(10) NOT NULL,   
  `nick` varchar(30) NOT NULL,   
  `data` varchar(200) NOT NULL,  
   PRIMARY KEY (`index`),   
   KEY `indx_date` (`date`),   
   KEY `indx_nick` (`nick`) 
 ) ENGINE=InnoDB AUTO_INCREMENT=4016741 DEFAULT CHARSET=utf8
 ```
