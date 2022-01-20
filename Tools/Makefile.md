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

伪目标并不是一个文件，只是一个标签。make无法生成它的依赖关系，也无法决定它是否执行。伪目标的取名不能和文件名重名，不然就失去了伪目标的意义了。

使用`.PHONY`来显式的指明一个目标是伪目标，不管它是不是一个文件，这个目标就是伪目标

```makefile
.PHONY: clean
clean:
    rm *.o temp
```

伪目标同样可以作为默认目标，为其指定依赖的文件，并将其放在第一个。

伪目标同样也可以称为依赖，有点像子程序。

### 多目标

多个目标可以同时依赖于一个文件，并且将其生成的命令大体类似，那么可以将其合并。使用自动化变量`$@`可以处理命令不同的情况。

```makefile
bigoutput littleoutput: text.g
    generate text.g -$(subst output,, $@) > $@
```

等价于

```makefile
bigoutput : text.g
    generate text.g -big > bigoutput
littleoutput : text.g
    generate text.g -little > littleoutput
```

`-$(subst outputmm $@)`中的`$`表示执行一个Makefile函数，函数名为subst，后面的为参数。`$@`表示目标的集合，就像一个数组，依次取出目标，并执行命令。

### 静态模式

静态模式可以更加容易地定义多目标的规则。

```makefile
<targets ...> : <target-pattern> : <prereq-patterns>
    <command>
    ...
```

- targets定义了一系列的目标文件，可以有通配符。是目标的一个集合
- target-pattern指明targets的模式
- prereq-patterns是目标的依赖模式，对target-pattern形成的模式再进行一次依赖目标的定义

```makefile
objects = foo.o bar.o

all: $(objects)

$(objects): %.o : %.c
    $(cc) -c $(CFLAGS) $< -o $@
```

目标从`$objects`获取，`%.o`表明所有以`.o`结尾的目标，依赖模式`%.c`取模式`%.o`的`%`，也就是`foo bar`，并为其加上`.c`的后缀，于是依赖魔表就是`foo.c bar.c`。而`$<`和`$@`是自动化变量，`$<`表示第一个依赖文件，`$@`表示目标集。

上面的规则等价于:

```makefile
foo.o: foo.c
    $(CC) -c $(CFLAGS) foo.c -o foo.o
bar.o: bar.c
    $(CC) -c $(CFLAGS) bar.c -o bar.o
```

```makefile
files = foo.elc bar.o lose.o

$(filter %.o,$(files)) : %.o: %.c
    $(CC) -c $(CFLAGS) $< -o $@
$(filter %.elc,$(files)) : %.elc: %.el
    emacs -f batch-byte-compile $<
```

`$(filter %.o,$(files))`表示调用Makefile的filter函数，过滤`$files`集，只要其中`%.o`的内容。

### 自动生成依赖性

大多数编译器支持`-M`参数，自动为源文件寻找头文件，并生成一个依赖关系。如果使用的是GNU的C/C++编译器，使用`-MM`参数，`-M`会把标准库的头文件也包含进来。

GNU建议把编译器为每一个源文件自动生成的依赖关系放到它的`.d`文件中。我们可以写出`.c`文件和`.d`文件的依赖关系，并让make自动更姓或生成`.d`文件，再把它包含在主makefile中。

一个生成`.d`文件的规则:

```makefile
%.d: %.c
    @set -e; rm -f $@; \
    $(CC) -M $(CPPFLAGS) $< > $@.$$$$; \
    sed 's, \($*\)\.o[ :]*,\1.o $@ :,g' < $@.$$$$ > $@; \
    rm -f $@.$$$$
```

这个规则的意思是，所有的`.d`文件依赖于`.c`文件，`rm -f $@`的意思是删除所有的目标文件，也就是`.d`文件。第二行的意思是，为每个依赖文件`$<`，也就是`.c`文件生成依赖文件，`$@`表示模式`%.d`文件，如果文件名是`name.c`，那么`%`就是`name`，`$$$$`是一个随机编号。生成的文件有可能是`name.d.12345`，第三行使用sed命令作一个替换，第四行删除临时文件。

然后可以把自动生成的规则加入到主Makefile中

```makefile
sources = foo.c bar.c
include $(sources:.c=.c)
```

`.c=.d`的意思是作一个替换，把变量`$(sources)`所有`.c`的字串都替换成`.d`。`include`是按次序来载入文件，最先载入的`.d`文件中的目标会称为默认目标。

## 书写命令
