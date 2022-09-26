# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - #
#							    	配  置       								   #
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - #

# 生成的目标文件
TARGET   		:= Pcpp
# 生成的目标文件类型 .out 可执行文件 .so 动态库 .a 静态库
TARGET_TYPE		:= .out
# 包含目录
DIR_INCS 		:= ../include/ ../include/a
# 源码目录
DIR_SRCS 		:= ./
# 链接库
DIR_LIBS 		:= ../bin/
# 临时文件目录
DIR_OBJ  		:= ../obj/
# 目标文件存放目录
DIR_BIN  		:= ../bin/
# 编译器
CX  			:= g++
# 编译参数
CFLAGS 			:= 
CXXFLAGS 		:= -g -Wall -fPIC 
#  链接器参数。（如：“-ld”）
LDFLAGS 		:= -m64

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - #
#							    	生  成       								   #
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - #

# 文件件目录
INC_DIR			:= $(addprefix -I,$(DIR_INCS))

# 递规源码目录 参数： -maxdepth 目录递归深度 -type d 目录，DIR_SRCS为./a ./b ./c 得到的数据类似（./a ./a/a1 ./b ./b/b1 ./c ./c/c1 ）
# 源文件目录
SRC_DIRS 		:= $(shell find $(DIR_SRCS) -maxdepth 10 -type d)

# $(foreach VAR,LIST,TEXT)	不展开先不展开TEXT，把LIST中使用空格分割的单词依次取出赋值给变量VAR，然后展开TEXT表达式。	空格分隔的多次表达式TEXT的展开结果。
# $(wildcard PATTERN)	列出当前目录下所有符合模式PATTERN格式的文件名。	空格分割的文件名列表
# 源码文件
SRC_FILES 		:= $(foreach dir,$(SRC_DIRS),$(wildcard $(dir)/*.cpp) $(wildcard $(dir)/*.c))
# 路径中的// 替换成/
SRC_FILES		:= $(subst //,/,$(SRC_FILES))
# 路径中的/./ 替换成/
SRC_FILES		:= $(subst /./,/,$(SRC_FILES))

# $(patsubst PATTERN,REPLACEMENT,TEXT)	搜索TEXT中以空格分开的单词，将否符合模式TATTERN的替换为REPLACEMENT。参数PATTERN和REPLACEMENT中可以使用模式通配符%。
# $(VAR:PATTERN=REPLACEMENT)相当于$(patsubst PATTERN,REPLACEMENT,$(VAR))，
# 而$(VAR:SUFFIX=REPLACEMENT)相当于$(patsubst %SUFFIX,%REPLACEMENT,$(VAR))。	替换后的新字符串。
# 生成临时文件
OBJS    		:= $(patsubst %.c,%.o,$(patsubst %.cpp,%.o,$(SRC_FILES)))
# 添加目录
OBJS			:= $(addprefix $(DIR_OBJ)/$(TARGET)/,$(OBJS))
# 路径中的// 替换成/
OBJS			:= $(subst //,/,$(OBJS))
# 路径中的// 替换成/
OBJS			:= $(subst /./,/,$(OBJS))

# 创建临时目录
$(foreach each,$(OBJS), $(shell mkdir -p $(dir $(each))))

# 输出
OUT_BIN 		:= $(subst //,/,$(DIR_BIN)/$(TARGET)) 
$(OUT_BIN)		: $(OBJS) 
	$(CX)  $(OBJS) -o $(subst //,/,$(OUT_BIN))  $(LDFLAGS) 


PRINTLOG		: 
	$(info $(SRC_DIRS) )
	$(info $(SRC_FILES) )
	$(info $(OBJS) )
	$(info $(INC_DIR) )

OBJCPP := $(subst //,/,$(DIR_OBJ)/$(TARGET)/%.o)
$(OBJCPP)		: %.cpp
	$(CX) -c -MMD -MP $(CFLAGS) $(CXXFLAGS) $(INC_DIR)  $< -o $@

OBJC := $(subst //,/,$(DIR_OBJ)/$(TARGET)/%.o)
$(OBJC)			: %.c
	$(CX) -c -MMD -MP $(CFLAGS) $(CXXFLAGS) $(INC_DIR)  $< -o $@
	
.PHONY: 		all clean

all:			$(OUT_BIN)

clean:
	# rm -f $(SRCS:.c=.o) $(TARGET)
	rm -rf $(subst //,/,$(DIR_OBJ)/$(TARGET))
	rm -f $(OUT_BIN)

-include $(patsubst %.o, %.d, $(OBJS) )

# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - #
#							    	资  料       								   #
# - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - #
# Linux命令文档
# https://wangchujiang.com/linux-command/hot.html
# Makefile参考文档
# https://seisman.github.io/how-to-write-makefile/implicit_rules.html
# https://blog.csdn.net/zhayushui/article/details/121998450
# https://www.chungkwong.cc/makefile.html
# http://blog.szm.me/misc/manage_dependencies_with_gcc_and_make/ (-MMD -MP参考这里)

#源文件，自动找所有.c和.cpp文件，并将目标定义为同名.o文件
#SOURCE  := $(wildcard *.c) $(wildcard *.cpp)
#OBJS    := $(patsubst %.c,%.o,$(patsubst %.cpp,%.o,$(SOURCE)))

#SUBDIRS := ./src
#RECURSIVE_MAKE := @for subdir in $(SUBDIRS); \
#        do \
#        make -C ?subdir || exit 1; \
#        done

#RECURSIVE_CLEAN := @for subdir in $(SUBDIRS); \
#        do \
#        ( cd ?subdir && make clean ) || exit 1; \
#        done

#all:
#	$(RECURSIVE_MAKE)

#clean:
#	$(RECURSIVE_CLEAN)


#.PHONY:all clean
# 排除目录
#exclude_dirs := .git
# 显示深度为1的子目录
#dirs := $(shell find . -type d -maxdepth 1)
# 去掉获取到目录名称前面的./
#dirs := $(basename $(patsubst ./%, %, $(dirs)))
# 过滤指定目录
#dirs := $(filter-out $(exclude_dirs), $(dirs))

#all:
#    $(foreach N,$(dirs),make -C $(N);)
#clean:
#    $(foreach N,$(dirs),make -C $(N) clean;)


# 规则语句前面是制表符TAB，写成空格会出错！

# 赋值符号：=基本赋值，:=覆盖之前的指，?=如果没有值则赋值，+=继续添加后面的值。
# x = foo
# y = $(x) bar  # 值为 xyz bar
# z:= $(x) bar  # 值为 foo bar
# x = xyz

# $@ – 目标文件
# $^ – 所有的依赖文件
# $< – 第一个依赖文件
# %.o:%.c – 表示把所有的[.c]文件都编译成[.o]文件

# -ldl，链接的就是libdl.so
# -L /usr/lib64/libdl.so

# 反斜杠（\）是换行符的意思。这样比较便于Makefile的易读


# 指令编译器和选项
# GCC编译选项CFLAGS参数
# 选项            说明
# -c              用于把源码文件编译成 .o 对象文件,不进行链接过程
# -o              用于连接生成可执行文件，在其后可以指定输出文件的名称
# -g              用于在生成的目标可执行文件中，添加调试信息，可以使用GDB进行调试
# -Idir           用于把新目录添加到include路径上，可以使用相对和绝对路径，“-I.”、“-I./include”、“-I/opt/include”
# -Wall           生成常见的所有告警信息，且停止编译，具体是哪些告警信息，请参见GCC手册，一般用这个足矣！
# -w              关闭所有告警信息
# -O              表示编译优化选项，其后可跟优化等级0\1\2\3，默认是0，不优化, 使用: -O1,-O2,-O3
# -fPIC           用于生成位置无关的代码
# -v              (在标准错误)显示执行编译阶段的命令，同时显示编译器驱动程序,预处理器,编译器的版本号
# -fopenmp        打开nmp并行编译选项,防止老版本的gcc不包含OpenMP的支持


# GCC链接选项LDFLAGS参数

# 选项              说明
# -llibrary        链接时在标准搜索目录中寻找库文件，搜索名为liblibrary.a 或 liblibrary.so
# -Ldir            用于把新目录添加到库搜索路径上，可以使用相对和绝对路径，“-L.”、“-L./include”、“-L/opt/include”
# -Wl,option       把选项 option 传递给连接器，如果 option 中含有逗号,就在逗号处分割成多个选项
# -static          使用静态库链接生成目标文件，避免使用共享库，生成目标文件会比使用动态链接库大
# -m32             生成32位的代码。
# -m64             生成64位的代码。

# = 是最基本的赋值                               #不推荐使用，容易搞混淆，不推荐使用
# := 是覆盖之前的值                              #表示直接赋值，赋予当前位置的值
# ?= 是如果没有被赋值过就赋予等号后面的值            #表示如果该变量没有被赋值，则赋予等号后的值
# += 是添加等号后面的值                           #将等号后面的值添加到前面的变量上

# '@' 符号的使用
#     通常makefile会将其执行的命令行在执行前输出到屏幕上。如果将‘@’添加到命令行前，这个命令将不被make回显出来。
# '-' 符号的使用     
#     通常删除，创建文件如果碰到文件不存在或者已经创建，那么希望忽略掉这个错误，继续执行，就可以在命令前面添加 -，
#     -rm dir；
#     -mkdir aaadir；
# '$' 符号的使用
#     美元符号$，主要扩展打开makefile中定义的变量
# '$$' 符号的使用
#     $$ 符号主要扩展打开makefile中定义的shell变量


# $@   表示规则中的目标文件集。在模式规则中，如果有多个目标，那么，"$@"就是匹配于目标中模式定义的集合。
# $%   仅当目标是函数库文件中，表示规则中的目标成员名。例如，如果一个目标是"foo.a(bar.o)"，那么，"$%"就是"bar.o"，
#      "$@"就是"foo.a"。如果目标不是函数库文件（Unix下是[.a]，Windows下是[.lib]），那么，其值为空。
# $<   依赖目标中的第一个目标名字。如果依赖目标是以模式（即"%"）定义的，那么"$<"将是符合模式的一系列的文件集。注意，其是一个一个取出来的。
# $?   所有比目标新的依赖目标的集合。以空格分隔。
# $^   所有的依赖目标的集合。以空格分隔。如果在依赖目标中有多个重复的，那个这个变量会去除重复的依赖目标，只保留一份。

# .PHONY  是一个伪目标，可以防止在Makefile中定义的只执行命令的目标和工作目录下的实际文件出现名字冲突，另一种是提交执行makefile时的效率。
# makefile 语法规则
# TARGETS : PREREQUISITES 
#      [Tab]COMMAND
#或
# TARGETS : PREREQUISITES；COMMAND


# addprefix       添加前缀
# addsuffix       添加后缀
# wildcard        获取文件名列表
# notdir          去除目录
# basename        去除后缀
# patsubst        模式替换
# info            显示变量
# lastword        获取路径无关的文件名

# makefile中的shell命令用法： $(shell shell_command...)
# dirname         获取文件路径（shell命令）
# pwd             获取当前路径（shell命令）

# 连接库
# -l  指定连接时期望连接的库的名字
# -L  指定连接库的搜索路径

# 参考：https://www.cnblogs.com/shaochuanhe/articles/14495457.html

# $(foreach <var>,<list>,<text>)   语法
#   这个函数的意思是，把参数<list>;中的单词逐一取出放到参数<var>;所指定的变量中，然后再执行< text>;所包含的表达式。
#   每一次<text>;会返回一个字符串，循环过程中，<text>;的所返回的每个字符串会以空格分隔，最后当整个循环结束时，<text>;
#   所返回的每个字符串所组成的整个字符串（以空格分隔）将会是foreach函数的返回值。
#   所以，<var>;最好是一个变量名，<list>;可以是一个表达式，而<text>;中一般会使用<var>;这个参数来依次枚举<list>;
#   中的单词。举个例子：
#       names := a b c d
#       files := $(foreach n,$(names),$(n).o)
#   上面的例子中，$(name)中的单词会被挨个取出，并存到变量“n”中，“$(n).o”每次根据“$(n)”计算出一个值，这些值以空格分隔，
#   最后作为foreach函数的返回，所以，$(files)的值是“a.o b.o c.o d.o”。

# $(filter PATTERN…,<text>) 
#    函数名称：过滤函数—filter。 
#    函数功能：过滤掉字串<text>中所有不符合模式“PATTERN”的单词，保留所有符合此模式的单词。可以使用多个模式。
#    模式中一般需要包含模式字符“%”。存在多个模式时，模式表达式之间使用空格分割。 
#    返回值：空格分割的<text>字串中所有符合模式“PATTERN”的字串。 
#    函数说明：“filter”函数可以用来去除一个变量中的某些字符串，

# $(filter-out <pattern>,<text>)
#   函数名称：反过滤函数，与filter函数功能相反
#   函数说明：函数的功能是功能和 filter 函数正好相反，但是用法相同。去除符合模式
#   pattern 的字符串，保留符合的字符串。返回值是保留的字符串。

# $(strip, <text>) 去空字符语句，去掉字串中开头和结尾的空字符（空字符包括空格、[Tab]等不可显示字符）。

# $(patsubst pattern, replacement, text)
#    寻找text中符合pattern格式的内容，用replacement代替他们。

#------------- compile using -------------------
# make clean
# make deep clean
# make solution=debug    #debug solution
# make                   #release solution -> TARGET_NAME
# make type=.a           #libtt_xxx.a
# make type=.so          #libtt_xxx.so
# make type=test         #lib test.exe


#------------- omp code using -------------------
# pragma omp parallel            //C,C++使用以#pragma omp开头的标准预处理指令
# pragma omp parallel for
#pragma omp parallel for num_threads(4)