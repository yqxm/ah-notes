- **验证异常**
	- **Junit4**
		- 使用注解`@Test`的`expected`属性。
		- ```Java
		  @Test(expected = NullPointerException.class)
		  public void whenExceptionThrown_thenExpectationSatisfied() {
		      String test = null;
		      test.length();
		  }
		  ```
	- **Junit5**
		- 使用`assertThrows()`方法
		- ```Java
		  @Test
		  public void whenExceptionThrown_thenAssertionSucceeds() {
		      Exception exception = assertThrows(NumberFormatException.class, () -> {
		          Integer.parseInt("1a");
		      });
		  }
		  ```