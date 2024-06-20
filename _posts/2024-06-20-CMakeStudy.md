# makefile笔记

[TOC]

## 关键字

### define

1. “define”定义变量的语法格式:从指示符“define”开始,“endif”结束,之间所有的内容就是所定义变量的值.索要定义的变量名字和指示符“define”在同一行,用空格分开;指示符所在下一行开始一直到“endif”所在行的上一行之间的若干行,都是变量值.

   ```makefile
   define two-lines
   echo foo
   echo $(bar)
   endif
   ```

   如果将变量“two-lines”作为命令包执行时，其相当于：`two-lines = echo foo; echo $(bar) `

2. 变量的风格：使用“define”定义的变量和使用“=”定义的变量一样，属于“递 归展开”式的变量，两者只是在语法上不同。因此“define”所定义的变量值中， 对其它变量或者函数引用不会在定义变量时进行替换展开，其展开是在“define” 定义的变量被展开的同时完成的.

3. 可以套嵌引用。因为是递归展开式变量，所以在嵌套引用时“$(x)”将是变量的 值的一部分.

4. 变量值中可以包含：换行符、空格等特殊符号（注意如果定义中某一行是以[Tab] 字符开始时，当引用此变量时这一行会被作为命令行来处理）

5. 可以使用“override”在定义时声明变量：这样可以**防止变量的值被命令行指定的值替代**:

   ```makefile
   override define two-lines 
   foo 
   $(bar) 
   endif 
   ```

   ​	





# Cmake Notebook



## 实践

> [!NOTE]
>
> 链接库

[cmake命令之target_include_directories-CSDN博客](https://blog.csdn.net/sinat_31608641/article/details/121713191)

```
./math/math.cmake
./CMakeLists.txt
```



> ./math/math.cmake

```cmake
cmake_minimum_required (VERSION 2.8)
 
project(MathFunctions)
 
# 查找当前目录下的所有源文件
# 并将名称保存到 DIR_LIB_SRCS 变量
aux_source_directory(. DIR_LIB_SRCS)

set(math_INCLUDE_DIRS
	math
)
 
# 指定生成 MathFunctions 链接库
add_library (MathFunctions ${DIR_LIB_SRCS})
target_include_directories(MathFunctions PRIVATE ${math_INCLUDE_DIRS})
```

> ./CMakeLists.txt

```cmake
# CMake 最低版本号要求
cmake_minimum_required (VERSION 2.8)
 
# 项目信息
project (Demo3)
 
# 添加 math 子目录,输出.a文件到output目录
include(math/math.cmake)
 
# 指定生成目标
add_executable(Demo main.cc)
 
# 添加链接库
target_link_libraries(Demo MathFunctions)
```





# Makefile

**对Makefile的理解**：简化C/C++项目编译过程（C++文件编译执行的四个阶段：预处理、编译、汇编、链接）。

在`Makefile`里可以执行对应`shell`的指令。

> [!NOTE]
>
> `<...>`  指代一类对象。

## 从一个Makefile实例开始

> 参考资料：
>
> - 源网址 「写给初学者的Makefile入门指南，知乎」：https://zhuanlan.zhihu.com/p/618350718
> - 跟我一起写 Makefile *[陈皓 [陈皓\]](https://zh.zlibrary-be.se/author/陈皓 [陈皓])*

Makefile 工具可以根据文件依赖，自动找出那些需要重新编译和链接的源文件，并对它们执行相应的动作。

### 前置知识

C/Cpp编译涉及的常见文件类型：

- `.c, .cpp, .cc`:源文件
- `.h, .hpp`:头文件
- `.d`: 依赖关系文件
- `.o`: 源文件编译结果
- 可执行文件
- ...

[**GCC指令介绍**](https://gcc.gnu.org/onlinedocs/gcc-11.4.0/gcc/Invoking-GCC.html)

```
gcc <src1.c> <...> -o <target> 
```

Makefile能检测文件修改，输出`gcc`指令。

gcc添加头文件路径：`-I<文件路径>`

**从源文件、头文件到可执行文件的过程：**

- 预处理：预处理器将以字符 `#` 开头的命令展开、插入到原始的C程序中。比如我们在源文件中能经常看到的、用于头文件包含的 `#include` 命令，它的功能就是告诉预编译器，将指定头文件的内容插入的程序文本中。

- 编译阶段：编译器将文本文件 `*.i` 翻译成文本文件 `*.s`，它包含一个汇编语言程序。
- 汇编阶段：汇编器将 `*.s` 翻译成机器语言指令，把这些指令打包成可重定位目标程序（relocatable object program）的格式，并保存在 `*.o` 文件中。
- 链接阶段：在 `bar.c` 中我们定义了 `Print_Progress_Bar` 函数，该函数会保存在目标文件 `bar.o` 中。直到链接阶段，链接器才以某种方式将 `Print_Progress_Bar` 函数合并到 `main` 函数中去。在链接时如果没有指定 `bar.o`，链接器就无法找到 `Print_Progress_Bar` 函数，也就会提示找不到相关函数的定义。



### 目标

**Makefile三要素**：目标（target）、依赖（prerequisite）和执行语句（recipe）：

```
目标: 依赖
	执行语句

main: main.c
	gcc main.c -o main
```

**伪目标：**

当前目录下有与目标同名的文件时，在没有依赖的情况下，Makefile 会认为目标文件已经是最新的状态了，目标下的语句也就不再执行。

为解决这一问题，我们可以将 `clean` 声明为伪目标，表明其并非是文件的命名。向特殊内置目标 `.PHONY` 添加 `clean` 依赖以达成这一目的：

```
.PHONY : clean

OUTPUT := ./output
clean:
        rm -r $(OUTPUT)
```



### 变量

**变量赋值：**

```
# 赋值，相当于C语言=
<variable> := ...

# 递归赋值：使用变量进行赋值时，会优先展开引用的变量
<variable> = $(variable2)			# 如果variable2是变量variable3的引用，则variable的值是variable3引用的值。

# 条件赋值：仅在变量没有定义时创建变量，已经定义过了，这句话相当于空语句
<variable> ?= ...

# 追加复制：在原信息上添加新信息
<variable> += ...
```

**使用变量**：`$(variable)`

**自动变量**：自动变量与普通变量不同，它不适用小括号

- `$<`指代依赖列表中的第一个依赖
- `$@`指代目标
- 



### 函数

**函数定义：**



**函数调用：**

语法：`$(<function> <arguments>)`，或者`${<function> <arguments>}`

**默认函数：**

- `wildcard`

  - 调用：`$(wildcard pattern…)`

  - 说明：返回当前路径下，所有符合`pattern`的文件

  - 示例：匹配当前路径下，所有的c文件，并且编译生成可执行文件

    ```
    # Makefile
    main : $(wildcard *.c)
            gcc $(wildcard *.c) -o main
    ```

- `foreach`

  - 调用：`$(foreach var,list,text)`
  - 说明：从 `list` 中逐个取出元素，赋值给 `var`，然后再展开 `text`。
  - 示例：`$(foreach dir,$(SUBDIR),$(dir)/*.c)`，输出`SUBDIR`内各个路径里的各个c文件。

- `patsubst`

  - 调用：`$(patsubst pattern,replacement,text)`
  - 说明：匹配 `text` 文本中与 `pattern` 模式相同的部分，并将匹配内容替换为 `replacement`。
  - 示例：`$(patsubst %.c,%.o,$(SRCS))`，得到c文件相同路径的o文件。

- `addprefix`

- `sort`

- `call`

- `dir`

- `filter`

#### 关键字

- `vpath`
- `include`
- `ifdef`, `endif`



### 其他特性

- 控制终端输出：在命令前使用`@`符号，可禁止 Makefile 将执行的命令输出至终端上：

- **自动生成依赖**

  `-MMD` 选项包含两个动作，一是生成依赖关系，二是保存依赖关系到 *.d 文件。与其类似的选项还有 `-MD`，其作用与 `-MMD` 相同，差别在于 `-MD` 选项会将系统头文件一同添加到依赖关系中。

- 错误忽略：`include` 前加了 `-` 符号，其作用是指示 make 在 include 操作出错时忽略这个错误，不输出任何错误信息并继续执行接下来的操作。



**技巧：**

1. 保存`.o`文件：
   1. 没有保存 `.o` 文件，这导致我们每次文件变动都要重新执行预处理、编译和汇编来得到目标文件，即使新得到的文件与旧文件完全没有差别（即编译用到的源文件没有任何变化，就跟`bar.c` 一样）。
   2. 有保存 `.o` 文件，则会遇到第二个问题，即依赖中没有指定头文件，这意味着只修改头文件的情况下，源文件不会重新编译得到新的可执行文件！



### 通用模板

```
ROOT := $(shell pwd)

SUBDIR := $(ROOT)
SUBDIR += $(ROOT)/func

TARGET := main
OUTPUT := ./output

INCS := $(foreach dir,$(SUBDIR),-I$(dir))
SRCS := $(foreach dir,$(SUBDIR),$(wildcard $(dir)/*.c))
OBJS := $(patsubst $(ROOT)/%.c,$(OUTPUT)/%.o,$(SRCS))
DEPS := $(patsubst %.o,%.d,$(OBJS))

$(TARGET) : $(OBJS)
        @echo linking...
        @gcc $(OBJS) -o $@
        @echo complete!

$(OUTPUT)/%.o : %.c
        @echo compile $<...
        @mkdir -p $(dir $@)
        @gcc -MMD -MP -c $(INCS) $< -o $@

.PHONY : clean

clean:
        @echo try to clean...
        @rm -r $(OUTPUT)
        @echo complete!

-include $(DEPS)
```



#### 一个G++模板

> 只支持cpp

```
ROOT = $(shell pwd)

SUBDIR = mysql OOP Task UI

TARGET = SimpleUI
OUTPUT = build

INCS = $(foreach dir, $(SUBDIR), -I$(dir))
SRCS = $(foreach dir, $(SUBDIR), $(wildcard $(dir)/*.c*))
OBJS = $(patsubst %.cpp, $(OUTPUT)/%.o, $(SRCS))
DEPS = $(patsubst %.o, %.d, $(OBJS))

CC = g++
# $(TARGET): $(SRCS)
# 	$(CC) -MMD -MP $(INCS) $(SRCS) -o $(OUTPUT)/$@

# $(OUTPUT)/%.o: %.c
# 	@echo complete @<...
# 	@mkdir -p $(dir $@)
# 	@$(CC) -c $(INCS) $< -o $@

$(TARGET) : $(OBJS)
	@echo linking...
	$(CC)  $(OBJS) -o $(OUTPUT)/$(TARGET)
	@echo complete!

$(OUTPUT)/%.o : %.cpp
# @echo compile $<...
	@mkdir -p $(dir $@)
	$(CC) -fPIC -shared $(INCS) $< -o $@
.PHONY : clean

clean:
	rm -rf $(OUTPUT)/

run :
	@$(OUTPUT)/$(TARGET)

test:
	@echo $(OBJS)
	@echo $(patsubst %.o, %.d, $(OBJS))


-include $(DEPS)
```

#### 支持c和c++

```
ROOT = $(shell pwd)

SUBDIR = mysql OOP Task UI

TARGET = SimpleUI
OUTPUT = build

INCS = $(foreach dir, $(SUBDIR), -I$(dir))
SRCS = $(foreach dir, $(SUBDIR), $(wildcard $(dir)/*.c*))
OBJS = $(patsubst %.cpp, $(OUTPUT)/%.opp, $(filter %.cpp, $(SRCS))) \
$(patsubst %.c, $(OUTPUT)/%.o, $(filter %.c, $(SRCS)))
DEPS = $(patsubst %.opp, %.d, $(filter %.opp, $(OBJS))) $(patsubst %.o, %.d, $(filter %.o, $(OBJS)))

CC = g++
# $(TARGET): $(SRCS)
# 	$(CC) -MMD -MP $(INCS) $(SRCS) -o $(OUTPUT)/$@

# $(OUTPUT)/%.o: %.c
# 	@echo complete @<...
# 	@mkdir -p $(dir $@)
# 	@$(CC) -c $(INCS) $< -o $@

$(TARGET) : $(OBJS)
	@echo linking...
	$(CC)  $(OBJS) -o $(OUTPUT)/$(TARGET)
	@echo complete!

$(OUTPUT)/%.opp : %.cpp
# @echo compile $<...
	@mkdir -p $(dir $@)
	$(CC) -fPIC -shared $(INCS) $< -o $@

$(OUTPUT)/%.o : %.c
# @echo compile $<...
	@mkdir -p $(dir $@)
	$(CC) -fPIC -shared $(INCS) $< -o $@
.PHONY : clean


clean:
	rm -rf $(OUTPUT)/

run :
	@$(OUTPUT)/$(TARGET)

test:
	@echo $(SRCS)
	@echo $(OBJS)

rebuild:
	rm -rf $(OUTPUT)/
	@make

-include $(DEPS)
```

#### 递归搜索

```makefile
# ENABLE AUTO LOAD; if not use, please commente next line. 
USE_AUTO_ADD = 1  
AUTO_PATH = Middlewares/LVGL/GUI APP Expand ...

# After C_SOURCES is defined
ifdef USE_AUTO_ADD
rwildcard = $(foreach d, $(wildcard $1*), $(call rwildcard,$d/,$2) \
						$(filter $2, $d))
# AUTO_PATH_C := $(call rwildcard, $(AUTO_PATH), %.c)
AUTO_PATH_C := $(foreach d, $(AUTO_PATH), $(call rwildcard, $d, %.c))
C_SOURCES += $(AUTO_PATH_C)

endif

# After C_INCLUDES is defined
ifdef USE_AUTO_ADD
# AUTO_PATH_H = $(call rwildcard, $(AUTO_PATH), %.h)
AUTO_PATH_H =  $(foreach d, $(AUTO_PATH), $(call rwildcard, $d, %.h))
C_INCLUDES += $(addprefix -I,$(sort $(dir $(AUTO_PATH_H))))
endif

```

