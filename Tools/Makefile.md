# Makefile

来源 [<<跟我一起写Makefile>>](https://seisman.github.io/how-to-write-makefile/index.html)

## 基础

```makefile
target ... : prerequisites ...
    command
    ...
    ...
```

- target: 目标文件，执行文件或者标签
- prerequisites: 生成target所依赖的文件或target
- command: target执行的命令

prerequisites中如果有一个以上的文件比target文件要新的话，command所定义的命令就会被执行。命令以一个`TAB`作为开头。

### 变量

```makefile
# 定义变量
val = a.o b.h \
    c.o d.o

# ${}使用变量
t : ${val}
    <command>
```

### 自动推导

make看到一个`.o`文件会自动把它的`.c`文件添加到依赖关系中。编译命令也可以自动推导出来

### 清空目标文件

```makefile
.PHONY : clean
clean :
    -rm edit $(objects)
```

`.PHONY`表示`clean`是一个伪目标。rm前面的减号表示忽略问题并继续执行命令

### 引用其他的Makefile

```makefile
# include <filename>
-include foo.make *.mk $(bar)
```

前面加减号表示暂时忽略错误

### Makefiles

类似于`include`，但不提示错误。不建议使用。

### 工作方式

- 读入所有的Makefile。
- 读入被include的其它Makefile。
- 初始化文件中的变量。
- 推导隐晦规则，并分析所有规则。
- 为所有的目标文件创建依赖关系链。
- 根据依赖关系，决定哪些目标要重新生成。
- 执行生成命令。
