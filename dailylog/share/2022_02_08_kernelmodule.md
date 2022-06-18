


2. linux 中module_init()加载顺序 
- https://blog.csdn.net/shafa00419/article/details/85234867
3. module_init机制的理解 https://blog.csdn.net/weixin_37571125/article/details/78665184　
模块代码有两种运行方式，一是静态编译连接进内核，在系统启动过程中进行初始化；一是编译成可动态加载的module，通过insmod动态加载重定位到内核。这两种方式可以在Makefile中通过obj-y或obj-m选项进行选择。

而一旦可动态加载的模块目标代码（.ko）被加载重定位到内核，其作用域和静态链接的代码是完全等价的。所以这种运行方式的优点显而易见：

- 可根据系统需要运行动态加载模块，以扩充内核功能，不需要时将其卸载，以释放内存空间；
- 当需要修改内核功能时，只需编译相应模块，而不必重新编译整个内核。

因为这样的优点，在进行设备驱动开发时，基本上都是将其编译成可动态加载的模块。但是需要注意，有些模块必须要编译到内核，随内核一起运行，从不卸载，如 vfs、platform_bus等。

那么同样一份C代码如何实现这两种方式的呢？
答案就在于module_init宏！下面我们一起来分析module_init宏。（这里所用的Linux内核版本为3.10.10）
定位到Linux内核源码中的 include/linux/init.h，可以看到有如下代码：
```c
#ifndef MODULE
// 省略
#define module_init(x)  __initcall(x);
// 省略
#else
 
#define module_init(initfn) \
    int init_module(void) __attribute__((alias(#initfn)));
// 省略
#endif
```
显然，MODULE 是由Makefile控制的。上面部分用于将模块静态编译连接进内核，下面部分用于编译可动态加载的模块。接下来我们对这两种情况进行分析。
方式一：#ifndef MODULE

代码梳理：

    #define module_init(x)  __initcall(x);
    |
    --> #define __initcall(fn) device_initcall(fn)
        |
        --> #define device_initcall(fn)     __define_initcall(fn, 6)
            |
            --> #define __define_initcall(fn, id) \
                    static initcall_t __initcall_##fn##id __used \
                    __attribute__((__section__(".initcall" #id ".init"))) = fn

即 module_init(hello_init) 展开为：

    static initcall_t __initcall_hello_init6 __used \
        __attribute__((__section__(".initcall6.init"))) = hello_init

这里的 initcall_t 是函数指针类型，如下：

typedef int (*initcall_t)(void);

　GNU编译工具链支持用户自定义section，所以我们阅读Linux源码时，会发现大量使用如下一类用法：
```
__attribute__((__section__("section-name"))) 
```
__attribute__用来指定变量或结构位域的特殊属性，其后的双括弧中的内容是属性说明，它的语法格式为：__attribute__ ((attribute-list))。它有位置的约束，通常放于声明的尾部且“ ;” 之前。
　　这里的attribute-list为__section__(“.initcall6.init”)。通常，编译器将生成的代码存放在.text段中。但有时可能需要其他的段，或者需要将某些函数、变量存放在特殊的段中，section属性就是用来指定将一个函数、变量存放在特定的段中。

　　所以这里的意思就是：定义一个名为 __initcall_hello_init6 的函数指针变量，并初始化为 hello_init（指向hello_init）；并且该函数指针变量存放于 .initcall6.init 代码段中。

　接下来，我们通过查看链接脚本（ arch/$(ARCH)/kernel/vmlinux.lds.S）来了解 .initcall6.init 段。
　　可以看到，.init段中包含 INIT_CALLS，它定义在include/asm-generic/vmlinux.lds.h。INIT_CALLS 展开后可得：

    #define INIT_CALLS                          \
            VMLINUX_SYMBOL(__initcall_start) = .;           \
            *(.initcallearly.init)                  \
            INIT_CALLS_LEVEL(0)                 \
            INIT_CALLS_LEVEL(1)                 \
            INIT_CALLS_LEVEL(2)                 \
            INIT_CALLS_LEVEL(3)                 \
            INIT_CALLS_LEVEL(4)                 \
            INIT_CALLS_LEVEL(5)                 \
            INIT_CALLS_LEVEL(rootfs)                \
            INIT_CALLS_LEVEL(6)                 \
            INIT_CALLS_LEVEL(7)                 \
            VMLINUX_SYMBOL(__initcall_end) = .;

进一步展开为：

            __initcall_start = .;           \
            *(.initcallearly.init)          \
            __initcall0_start = .;          \
            *(.initcall0.init)              \
            *(.initcall0s.init)             \
            // 省略1、2、3、4、5
            __initcallrootfs_start = .;     \
            *(.initcallrootfs.init)         \
            *(.initcallrootfss.init)            \
            __initcall6_start = .;          \
            *(.initcall6.init)              \
            *(.initcall6s.init)             \
            __initcall7_start = .;          \
            *(.initcall7.init)              \
            *(.initcall7s.init)             \
            __initcall_end = .;

　上面这些代码段最终在kernel.img中按先后顺序组织，也就决定了位于其中的一些函数的执行先后顺序（__initcall_hello_init6 位于 .initcall6.init 段中）。.init 或者 .initcalls 段的特点就是，当内核启动完毕后，这个段中的内存会被释放掉。这一点从内核启动信息可以看到：

Freeing unused kernel memory: 124K (80312000 - 80331000)

　那么存放于 .initcall6.init 段中的 __initcall_hello_init6 是怎么样被调用的呢？我们看文件 init/main.c，代码梳理如下：

    start_kernel
    |
    --> rest_init
        |
        --> kernel_thread
            |
            --> kernel_init
                |
                --> kernel_init_freeable
                    |
                    --> do_basic_setup
                        |
                        --> do_initcalls
                            |
                            --> do_initcall_level(level)
                                |
                                --> do_one_initcall(initcall_t fn)

kernel_init 这个函数是作为一个内核线程被调用的（该线程最后会启动第一个用户进程init）。
我们着重关注 do_initcalls 函数，如下：

    static void __init do_initcalls(void)
    {
        int level;
     
        for (level = 0; level < ARRAY_SIZE(initcall_levels) - 1; level++)
            do_initcall_level(level);
    }

函数 do_initcall_level 如下：

    static void __init do_initcall_level(int level)
    {
        // 省略
        for (fn = initcall_levels[level]; fn < initcall_levels[level+1]; fn++)
            do_one_initcall(*fn);
    }

函数 do_one_initcall 如下：

    int __init_or_module do_one_initcall(initcall_t fn)
    {
        int ret;
        // 省略
        ret = fn();
        return ret;
    }

initcall_levels 的定义如下：

    static initcall_t *initcall_levels[] __initdata = {
        __initcall0_start,
        __initcall1_start,
        __initcall2_start,
        __initcall3_start,
        __initcall4_start,
        __initcall5_start,
        __initcall6_start,
        __initcall7_start,
        __initcall_end,
    };

initcall_levels[] 中的成员来自于 INIT_CALLS 的展开，如“__initcall0_start = .;”，这里的 __initcall0_start是一个变量，它跟代码里面定义的变量的作用是一样的，所以代码里面能够使用__initcall0_start。因此在 init/main.c 中可以通过 extern 的方法将这些变量引入，如下：

    extern initcall_t __initcall_start[];
    extern initcall_t __initcall0_start[];
    extern initcall_t __initcall1_start[];
    extern initcall_t __initcall2_start[];
    extern initcall_t __initcall3_start[];
    extern initcall_t __initcall4_start[];
    extern initcall_t __initcall5_start[];
    extern initcall_t __initcall6_start[];
    extern initcall_t __initcall7_start[];
    extern initcall_t __initcall_end[];

　到这里基本上就明白了，在 do_initcalls 函数中会遍历 initcalls 段中的每一个函数指针，然后执行这个函数指针。因为编译器根据链接脚本的要求将各个函数指针链接到了指定的位置，所以可以放心地用 do_one_initcall(*fn) 来执行相关初始化函数。
　　
　　我们例子中的 module_init(hello_init) 是 level6 的 initcalls 段，比较靠后调用，很多外设驱动都调用 module_init 宏，如果是静态编译连接进内核，则这些函数指针会按照编译先后顺序插入到 initcall6.init 段中，然后等待 do_initcalls 函数调用。

方式二：#else

相关代码：

    #define module_init(initfn)                 \
        static inline initcall_t __inittest(void)       \
        { return initfn; }                  \
        int init_module(void) __attribute__((alias(#initfn)));

　__inittest 仅仅是为了检测定义的函数是否符合 initcall_t 类型，如果不是 __inittest 类型在编译时将会报错。所以真正的宏定义是：

    #define module_init(initfn)                 \
        int init_module(void) __attribute__((alias(#initfn)));

因此，用动态加载方式时，可以不使用 module_init 和 module_exit 宏，而直接定义 init_module 和 cleanup_module 函数，效果是一样的。
　　
　　alias 属性是 gcc 的特有属性，将定义 init_module 为函数 initfn 的别名。所以 module_init(hello_init) 的作用就是定义一个变量名 init_module，其地址和 hello_init 是一样的。
　　
　　上述例子编译可动态加载模块过程中，会自动产生 HelloWorld.mod.c 文件，内容如下：

    #include <linux/module.h>
    #include <linux/vermagic.h>
    #include <linux/compiler.h>
     
    MODULE_INFO(vermagic, VERMAGIC_STRING);
     
    struct module __this_module
    __attribute__((section(".gnu.linkonce.this_module"))) = {
        .name = KBUILD_MODNAME,
        .init = init_module,
    #ifdef CONFIG_MODULE_UNLOAD
        .exit = cleanup_module,
    #endif
        .arch = MODULE_ARCH_INIT,
    };
     
    static const char __module_depends[]
    __used
    __attribute__((section(".modinfo"))) =
    "depends=";

可知，其定义了一个类型为 module 的全局变量 __this_module，成员 init 为 init_module（即 hello_init），且该变量链接到 .gnu.linkonce.this_module 段中。

　编译后所得的 HelloWorld.ko 需要通过 insmod 将其加载进内核，由于 insmod 是 busybox 提供的用户层命令，所以我们需要阅读 busybox 源码。代码梳理如下：（文件 busybox/modutils/ insmod.c）

    insmod_main
    |
    --> bb_init_module
        |
        --> init_module

而 init_module 定义如下：（文件 busybox/modutils/modutils.c）

#define init_module(mod, len, opts) syscall(__NR_init_module, mod, len, opts)

因此，该系统调用对应内核层的 sys_init_module 函数。

回到Linux内核源代码（kernel/module.c），代码梳理：

    SYSCALL_DEFINE3(init_module, ...)
    |
    -->load_module
        |
        --> do_init_module(mod)
            |
            --> do_one_initcall(mod->init);

文件（include/linux/syscalls.h）中，有：

#define SYSCALL_DEFINE3(name, ...) SYSCALL_DEFINEx(3, _##name, __VA_ARGS__)

从而形成 sys_init_module 函数。

模块释放

    init/main.c中
     
    start_kernel
    |
    --> rest_init
        |
        --> kernel_init
            |
            -->free_initmem