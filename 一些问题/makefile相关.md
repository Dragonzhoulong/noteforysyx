```bash
LIBS += $(if $(CONFIG_TARGET_NATIVE_ELF),-lreadline -ldl -pie,)
```

### $
>注重 $ 的作用

在 `Makefile` 中，`$` 符号有特殊的含义，它通常用于表示变量引用、命令替换和函数调用等。以下是 `Makefile` 中 `$` 符号的几种常见用法：

1. **变量引用：** 在`Makefile`中，我们可以使用变量来保存和管理数据。使用`$`符号可以引用这些变量的值。例如，如果有一个变量`CC`，它表示编译器的名称，我们可以使用`$(CC)`来引用这个变量，如：`$(CC) file.c -o output`。
    
2. **命令替换：** 通过使用反引号（`）`或`$()`，可以在`Makefile`中执行命令，并将其输出作为一个变量的值。例如，可以通过`$(shell command)`来执行命令并将输出保存到一个变量中，如：`FILES = $(shell ls)`，这将把当前目录下的文件列表保存在变量`FILES`中。
    
3. **自动化变量：** 在规则中，`$@`表示目标文件的名称，`$<`表示第一个依赖文件的名称，`$^`表示所有依赖文件的列表。例如，下面的规则将会把`file.c`编译为`file.o`：`file.o: file.c`，`$(CC) -c $< -o $@`。
    
4. **函数调用：** `Makefile`中有一些内置的函数，可以使用`$()`语法来调用这些函数。例如，`$(wildcard pattern)`函数可以用来匹配文件路径模式，返回匹配的文件列表。
    
5. **转义字符：** 如果想要在`Makefile`中使用`$`符号本身而不是其特殊含义，可以通过`$$`进行转义。例如，`$$`将会被解释为单个`$`字符。
    

---------------
### LIBS 的作用域
implict rule 的作用?


















-------------
#### $代码讲解
请注意，`Makefile`中的`$`符号的含义可能会因不同的`Makefile`解释器（如GNU Make、BSD Make等）以及版本而有所不同，因此在编写`Makefile`时要小心处理`$`符号的使用，以确保得到正确的结果。

这是一个 `Makefile` 中的变量定义，它的含义如下：

1. `LIBS +=`: 这表示将后面的内容添加到名为 `LIBS` 的变量中。`+=` 运算符是用来追加内容而不是替换。
    
2. `$(if $(CONFIG_TARGET_NATIVE_ELF), ... , ...)`: 这是一个条件表达式。它检查名为 `CONFIG_TARGET_NATIVE_ELF` 的变量是否已定义且非空。如果 `CONFIG_TARGET_NATIVE_ELF` 已定义且非空，它会执行第一个逗号前的内容；否则，执行第一个逗号后的内容。
    
3. `-lreadline -ldl -pie,`: 这是条件表达式的两个分支之一。如果 `CONFIG_TARGET_NATIVE_ELF` 已定义且非空，那么将会添加 `-lreadline -ldl -pie` 到 `LIBS` 变量中。
    

让我们解释一下：

假设 `CONFIG_TARGET_NATIVE_ELF` 已经定义且非空，那么 `LIBS` 变量将被扩展为 `$(LIBS) -lreadline -ldl -pie`，也就是在原来 `LIBS` 变量的基础上，追加了 `-lreadline -ldl -pie` 这些内容。

这里的目的是根据条件是否为原生 ELF 目标来选择性地链接 `readline` (`-lreadline`)、动态链接器 (`-ldl`) 以及生成位置无关代码 (`-pie`) 到可执行文件中。这样可以根据编译目标的不同来添加不同的链接选项，以确保生成符合目标平台的可执行文件。

请注意，该段代码只是一个片段，可能在实际的 `Makefile` 文件中有更多的内容和逻辑。要正确理解这段代码的作用，需要了解 `Makefile` 文件的完整内容和相关的变量定
义。


1. `TOPNAME = top` : 定义了顶层模块的名称为"top"。
    
2. `NXDC_FILES = constr/top.nxdc`: 定义了约束文件的路径为"constr/top.nxdc"。
    
3. `VERILATOR = verilator`: 定义了Verilator仿真工具的名称为"verilator"。
    
4. `VERILATOR_CFLAGS`: 定义了Verilator编译选项，包括一些优化选项和其他标志。
    
5. `BUILD_DIR = ./build`和`OBJ_DIR = $(BUILD_DIR)/obj_dir`: 定义了构建目录和对象目录的路径。
    
6. `BIN = $(BUILD_DIR)/$(TOPNAME)`: 定义了生成的可执行文件的路径和名称。
    
7. `default: $(BIN)`: 默认目标是构建可执行文件。
    
8. `$(shell mkdir -p $(BUILD_DIR))`: 使用`mkdir -p`命令在构建目录创建子目录。
    
9. `SRC_AUTO_BIND`: 定义了一个自动生成绑定源文件的规则。
    
10. `VSRCS`和`CSRCS`: 分别定义了Verilog和C/C++源文件的路径。
    
11. `include $(NVBOARD_HOME)/scripts/nvboard.mk`: 包含了"nvboard.mk"文件，可能是用于NVBoard项目的构建规则。
    
12. `INCFLAGS`、`CFLAGS`和`LDFLAGS`: 分别定义了头文件路径、C/C++编译选项和链接选项。
    
13. `$(BIN): $(VSRCS) $(CSRCS) $(NVBOARD_ARCHIVE)`: 定义了可执行文件的依赖关系。
    
14. `all: default`: 定义了"all"目标，它依赖于"default"目标。
    
15. `run: $(BIN)`: 定义了"run"目标，用于运行生成的可执行文件。
    
16. `clean`: 定义了"clean"目标，用于删除生成的构建目录和对象文件。
    
17. `.PHONY: default all clean run`: 声明了一些伪目标，告诉Makefile这些目标不代表真实的文件。