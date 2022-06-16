- ```C
  #define ROW 12
  #define COL 10
  
  int main() {
    char **dd = malloc(sizeof(char *) * ROW);
    
    char sd[ROW][COL];
  
    for (int i = 0; i < ROW; ++i) {
      dd[i] = malloc(sizeof(char) * COL);
      strcpy(dd[i], "123");;
    }
  
  
    for (int i = 0; i < ROW; ++i) {
      free(dd[i]);
    }
    free(dd);
  }
  ```
- 对上面这个例子来说，不是连续的。
- `dd`是一个存储指针的数组。里面的指针大小相差为`0x20`。
- `sd`就是一个二维数组，`sd[0]`和`sd[1]`地址之间相差10，即`0xa`。