# toolchain-build-ck804

Linux 系统下构建工具链，请直接查看 README.md 。

Windows 系统下构建工具链，请根据 Linux 系统下工具链构建方法自行琢磨用法（一般而言，并无构建必要）。

macOS 系统下构建工具链，请参考下文。

## macOS 系统构建工具链

### 准备系统环境

macOS 系统构建工具链，全程在终端中操作，建议先安装好 `Homebrew`和`MacPorts`工具，以便安装下面的依赖。

关于 `Homebrew`和`MacPorts`如何安装，不在本文探讨范围，请自行解决。

接下来，

先使用 `MacPorts` 工具安装下面的依赖：

    % sudo port install autoconf automake bc bison curl expat flex gawk gperf libiconv libtool patchutils texinfo zlib

再使用 `Homebrew` 工具安装下面的依赖：

    % brew install gmp libmpc mpc mpfr

> 注意：安装过程中，根据不同的系统和不同的工具环境，可能会出现不同的错误，需要根据自己的环境自行解决，包括下面的构建步骤中出现的错误也一样，建议使用 brew 和 port 交替使用以解决某些问题。**如果不具备这方面能力**，建议不要尝试构建，直接使用 `Windows` 或 `Linux` 系统现成的工具链是最好的选择。

### 下载代码

首先下载克隆仓库代码

```
% git clone https://github.com/wdyichen/toolchain-build-ck804
```

然后更新各模块

```
% cd toolchain-build-ck804
% git submodule update --init
```

> 1.  如果想减少下载量，可改变克隆命令为：`% git clone --depth 1 https://github.com/wdyichen/toolchain-build-ck804`，下载后大概为 **2.5G** 左右。
> 2. 为了能够完成下面的构建，需要硬盘至少有 **46G** 空闲空间。

### 构建工具链
执行下面的命令即可启动构建：

`./build-csky-gcc.py csky-gcc --src ./ --triple  csky-unknown-elf --cpu ck804ef --fpu hard --endian little --build-type Release`

> 如果想进一步提升构建速度，可将构建命令改为 `./build-csky-gcc.py csky-gcc --src ./ --triple  csky-unknown-elf --cpu ck804ef --fpu hard --endian little --build-type Release --no-multilib --jobs=-1`。

## 构建结果

如果构建过程没有错误，则构建结果可以在如下路径找到：

`build-gcc-csky-unknown-elf/Xuantie-800-gcc-elf-newlib-x86_64`

构建结果的结构大致如下：

```
Xuantie-800-gcc-elf-newlib-x86_64
├── bin
│   ├── csky-unknown-elf-addr2line
│   ├── csky-unknown-elf-ar
│   ├── csky-unknown-elf-as
│   ├── csky-unknown-elf-c++
│   ├── csky-unknown-elf-c++filt
│   ├── csky-unknown-elf-cpp
│   ├── csky-unknown-elf-elfedit
│   ├── csky-unknown-elf-g++
│   ├── csky-unknown-elf-gcc
│   ├── csky-unknown-elf-gcc-13.0.1
│   ├── csky-unknown-elf-gcc-ar
│   ├── csky-unknown-elf-gcc-nm
│   ├── csky-unknown-elf-gcc-ranlib
│   ├── csky-unknown-elf-gcov
│   ├── csky-unknown-elf-gcov-dump
│   ├── csky-unknown-elf-gcov-tool
│   ├── csky-unknown-elf-gdb
│   ├── csky-unknown-elf-gdb-add-index
│   ├── csky-unknown-elf-gprof
│   ├── csky-unknown-elf-ld
│   ├── csky-unknown-elf-ld.bfd
│   ├── csky-unknown-elf-lto-dump
│   ├── csky-unknown-elf-nm
│   ├── csky-unknown-elf-objcopy
│   ├── csky-unknown-elf-objdump
│   ├── csky-unknown-elf-ranlib
│   ├── csky-unknown-elf-readelf
│   ├── csky-unknown-elf-size
│   ├── csky-unknown-elf-strings
│   └── csky-unknown-elf-strip
├── csky-unknown-elf
│   ├── bin
│   ├── include
│   ├── lib
│   └── sys-include
├── include
│   ├── ctf-api.h
│   ├── ctf.h
│   ├── gdb
│   ├── sframe-api.h
│   ├── sframe.h
│   └── sim
├── lib
│   ├── bfd-plugins
│   ├── gcc
│   ├── libcc1.0.so
│   ├── libcc1.la
│   ├── libcc1.so -> libcc1.0.so
│   ├── libctf-nobfd.a
│   ├── libctf-nobfd.la
│   ├── libctf.a
│   ├── libctf.la
│   ├── libsframe.a
│   └── libsframe.la
├── libexec
│   └── gcc
├── share
│   ├── gcc-13.0.1
│   ├── gdb
│   ├── info
│   ├── locale
│   └── man
└── x86_64-apple-darwin21.6.0
    └── csky-unknown-elf
```

由此可知构建生成的工具链的前缀为 `csky-unknown-elf-`，如 gcc 则为 `csky-unknown-elf-gcc`。
