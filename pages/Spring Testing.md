- 标签
	- `@Sql` 配置要运行的`SQL`脚本或者单行`SQL`语句
		- 方法上的`@Sql`会覆盖类名上的`@Sql`
		- 脚本在`resources`文件夹下需要添加`classpath`，`classpath`和目录之间不能有空格。
			- ```java
			  @Sql("classpath:db/init.sql")
			  ```
		- `scripts`变量指定要运行的脚本，`statements`变量指定要运行的语句
			- ```Java
			  @Sql(scripts = "classpath:db/init.sql", 
			       statements = "INSERT INTO sentence (content, origin, author, language, create_time, update_time)" +
			       	" VALUES ('test content', 'test origin', 'test author', 'english', now(), now())")
			  ```