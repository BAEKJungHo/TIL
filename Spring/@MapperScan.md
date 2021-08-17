# @MapperScan

MapperScan 에서 scan 을 진행할 Annotation 클래스를 지정하고 basePacke 를 설정해준다. 

지정된 Annotation 이 설정되어 있는 클래스는 scan 을 진행하고 해당 클래스들은 아래의 SqlSessionFactoryBean 을 주입 받는다. 

여기서 중요한 포인트는 SqlSessionFactoryBean 해당 클래스라 할 수 있다. 

```java
@Configuration
@MapperScan(basePackages = "net.weave.*.*.*.repositoryMysql", sqlSessionFactoryRef="mysqlSqlSessionFactory")
@EnableTransactionManagement
public class MysqlConfig {

    @Bean(name ="mysqlDataSource")
    @ConfigurationProperties(prefix ="mysql.datasource")
    public DataSource mysqlDataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean(name ="mysqlSqlSessionFactory")
    public SqlSessionFactory mysqlSqlSessionFactory(
            @Qualifier("mysqlDataSource") DataSource mysqlDataSource,
            ApplicationContext applicationContext,
            @Value("${mybatis.type-aliases-package}") String typeAliasesPackage,
            @Value("${mysql.mybatis.mapper-locations}") String mapperLocations,
            @Value("${mybatis.config-location}") String configLocation
    ) throws Exception {
        SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
        sqlSessionFactoryBean.setDataSource(mysqlDataSource);
        sqlSessionFactoryBean.setMapperLocations(applicationContext.getResources(mapperLocations));
        sqlSessionFactoryBean.setTypeAliasesPackage(typeAliasesPackage);
        sqlSessionFactoryBean.setConfigLocation(applicationContext.getResource(configLocation));
        return sqlSessionFactoryBean.getObject();
    }

    @Bean(name ="mysqlSqlSessionTemplate")
    public SqlSessionTemplate mysqlSqlSessionTemplate(SqlSessionFactory mysqlSqlSessionFactory)throws Exception {
        return new SqlSessionTemplate(mysqlSqlSessionFactory);
    }

}
```

