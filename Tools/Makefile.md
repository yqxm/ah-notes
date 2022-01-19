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

## 书写规则

语法：

```makefile
targets : prerequisites
    command
    ...
# 或者
targets : prerequisites ; command
    command
    ...
```

command是命令行，如果其不与`target:prerequisites`在一行，那么，必须以`Tab`键开头，如果和`prerequisites`在一行，那么可以用分号做为分隔。

如果命令行太长可以使用`\`作为换行符

### 通配符

- `*`表示任意长度的字符串
- `~`表示宿主目录
- `?`

### 文件搜寻

#### VPATH特殊变量

`VPATH`关键字指定当前目录搜索不到目标文件时搜索的目录。

```makefile
# 两个目录，一个src,一个headers
VPATH = src:../headers
```

目录由冒号分隔

#### vpath关键字

和`VPATH`变量类似，但更为灵活，可以指定不同的文件在不同的搜索模式中。

有三种模式：

- `vpath <pattern> <directories>` 为符合模式`<pattern>`的文件指定搜索目录`<directories>`
- `vpath <pattern>` 清除复合模式`<pattern>`的文件的搜索目录。
- `vpath` 清除所有已被设置好了的文件搜索目录

`<pattern>`需要包含`%`字符。意思是匹配零或若干字符。

可以连续地使用vpath语句，以指定不同的搜索策略。如果连续的vpath中出现了相同或是重复的`<pattern>`，make会按照vpath语句的先后顺序来执行搜索。

```makefile
# 使用冒号来分隔目录
vpath %.c foo:doo
vpath %   blish
vpath %.c bar
```

### 伪目标


