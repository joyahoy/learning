<h1>NEMU</h1>
1.nemu是什么？上述描述对你来说也许还有些晦涩难懂, 让我们来看一个ATM机的例子.

ATM机是一个物理上存在的机器, 它的功能需要由物理电路和机械模块来支撑. 例如我们在ATM机上进行存款操作的时候, ATM机都会吭哧吭哧地响, 让我们相信确实是一台真实的机器. 另一方面, 现在第三方支付平台也非常流行, 例如支付宝. 事实上, 我们可以把支付宝APP看成一个模拟的ATM机, 在这个模拟的ATM机里面, 真实ATM机具备的所有功能, 包括存款, 取款, 查询余额, 转账等等, 都通过支付宝APP这个程序来实现.

同样地, NEMU就是一个模拟出来的计算机系统, 物理计算机中的基本功能, 在NEMU中都是通过程序来实现的. 要模拟出一个计算机系统并没有你想象中的那么困难. 我们可以把计算机看成由若干个硬件部件组成, 这些部件之间相互协助, 完成"运行程序"这件事情. 在NEMU中, 每一个硬件部件都由一个程序相关的数据对象来模拟, 例如变量, 数组, 结构体等; 而对这些部件的操作则通过对相应数据对象的操作来模拟. 例如NEMU中使用数组来模拟内存, 那么对这个数组进行读写则相当于对内存进行读写.

我们可以把实现NEMU的过程看成是开发一个支付宝APP. 不同的是, 支付宝具备的是真实ATM机的功能, 是用来交易的; 而NEMU具备的是物理计算机系统的功能, 是用来执行程序的. 因此我们说, NEMU是一个用来执行其它程序的程序.

<p>
  2.目前我们只需要关心NEMU子项目中的内容, 其它子项目会在将来进行介绍. NEMU主要由4个模块构成: monitor, CPU, memory, 设备.
</p>
<p>
  Monitor(监视器)模块是为了方便地监控客户计算机的运行状态而引入的. 它除了负责与GNU/Linux进行交互(例如读入客户程序)之外, 还带有调试器的功能, 为NEMU的调试提供了方便的途径. 从概念上来说, monitor并不属于一个计算机的必要组成部分, 但对NEMU来说, 它是必要的基础设施. 如果缺少monitor模块, 对NEMU的调试将会变得十分困难.
</p>

```
nemu
├── configs                    # 预先提供的一些配置文件
├── include                    # 存放全局使用的头文件
│   ├── common.h               # 公用的头文件
│   ├── config                 # 配置系统生成的头文件, 用于维护配置选项更新的时间戳
│   ├── cpu
│   │   ├── cpu.h
│   │   ├── decode.h           # 译码相关
│   │   ├── difftest.h
│   │   └── ifetch.h           # 取指相关
│   ├── debug.h                # 一些方便调试用的宏
│   ├── device                 # 设备相关
│   ├── difftest-def.h
│   ├── generated
│   │   └── autoconf.h         # 配置系统生成的头文件, 用于根据配置信息定义相关的宏
│   ├── isa.h                  # ISA相关
│   ├── macro.h                # 一些方便的宏定义
│   ├── memory                 # 访问内存相关
│   └── utils.h
├── Kconfig                    # 配置信息管理的规则
├── Makefile                   # Makefile构建脚本
├── README.md
├── resource                   # 一些辅助资源
├── scripts                    # Makefile构建脚本
│   ├── build.mk
│   ├── config.mk
│   ├── git.mk                 # git版本控制相关
│   └── native.mk
├── src                        # 源文件
│   ├── cpu
│   │   └── cpu-exec.c         # 指令执行的主循环
│   ├── device                 # 设备相关
│   ├── engine
│   │   └── interpreter        # 解释器的实现
│   ├── filelist.mk
│   ├── isa                    # ISA相关的实现
│   │   ├── mips32
│   │   ├── riscv32
│   │   ├── riscv64
│   │   └── x86
│   ├── memory                 # 内存访问的实现
│   ├── monitor
│   │   ├── monitor.c
│   │   └── sdb                # 简易调试器
│   │       ├── expr.c         # 表达式求值的实现
│   │       ├── sdb.c          # 简易调试器的命令处理
│   │       └── watchpoint.c   # 监视点的实现
│   ├── nemu-main.c            # 你知道的...
│   └── utils                  # 一些公共的功能
│       ├── log.c              # 日志文件相关
│       ├── rand.c
│       ├── state.c
│       └── timer.c
└── tools                      # 一些工具
    ├── fixdep                 # 依赖修复, 配合配置系统进行使用
    ├── gen-expr
    ├── kconfig                # 配置系统
    ├── kvm-diff
    ├── qemu-diff
    └── spike-diff
```

<p>
  3.为了支持不同的ISA, 框架代码把NEMU分成两部分: <strong>ISA无关的基本框架和ISA相关的具体实现</strong>. NEMU把ISA相关的代码专门放在nemu/src/isa/目录下, 并通过nemu/include/isa.h提供ISA相关API的声明. 这样以后, nemu/src/isa/之外的其它代码就展示了NEMU的基本框架. 这样做有两点好处:<br>
1.有助于我们认识不同ISA的共同点: 无论是哪种ISA的客户计算机, 它们都具有相同的基本框架<br>2.体现抽象的思想: 框架代码将ISA之间的差异抽象成API, 基本框架会调用这些API, 从而无需关心ISA的具体细节. 如果你将来打算选择一个不同的ISA来进行二周目的攻略, 你就能明显体会到抽象的好处了: 基本框架的代码完全不用修改!
</p>
<p><a href="https://nju-projectn.github.io/ics-pa-gitbook/ics2024/nemu-isa-api.html" target="_blank">这个页面</a>对上述API进行了整理, 供将来查阅使用, 目前你无需完全明白它们的作用.
"抽象"是计算机系统中一个非常重要的概念, 如果你现在不明白抽象的意义,
不必担心, 在PA的后续内容中, 你会一次又一次地遇到它.</p>
