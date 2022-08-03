- `@MybatisTest`
	- 默认情况下的配置
		- `mybatis`组件(`SqlSessionFactory`和`SqlSessionTemplate`)
		- 映射接口
		- 内存数据库
	- 如果测试的是某个`DAO`类，可以标注`@MybatisTest`和`@Import`
		- ```Java
		  @MybatisTEst
		  @Import(CityDao.class)
		  public class CityDaoTest {
		    	// ...
		  }
		  ```
- `@AutoConfigureTEstDatabase`更换数据库
	- ```Java
	  @MybatisTest
	  @AutoConfigureTestDatabase(replace = AutoConfigureTestDatabase.Replace.NONE)
	  public class CityMapperTest {
	      // ...
	  }
	  ```
- 如果要和其他`@***Test`一起使用，更换使用`@AutoConfigure Mybatis`，因为`@***Test`在同一个测试中不能出现多次。
-