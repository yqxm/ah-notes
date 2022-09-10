- #+BEGIN_CAUTION
  不要通过连接创建 unicode字符
  #+END_CAUTION
- C标准允许unicode字符作为标识符，字符常量和字符串字面量来进行命名。但C标准又说如果一个字符序列和字符连接(token concatenation)产生的unicode字符匹配的话，该行为是未定义的。
### 反例
	- ```C
	  #define assign(uc1, uc2, val) uc1##uc2 = val
	  
	  void func(void) {
	  	int \u0401;
	  	/* ... */
	  	assign(\u04, 01, 4);
	  	/* ... */
	  }
	  ```
### 正例
	- ```C
	  #define assign(ucn, val) ucn = val
	  
	  int main() {
	    int \u0401;
	  
	    assign(\u0401, 4);
	    printf("%d", \u0401);
	  }
	  ```