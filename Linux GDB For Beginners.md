https://linuxconfig.org/gdb-debugging-tutorial-for-beginners


```c
int actual_calc(int a, int b){
  int c;
  c=a/b;
  return 0;
}

int calc(){
  int a;
  int b;
  a=13;
  b=0;
  actual_calc(a, b);
  return 0;
}

int main(){
  calc();
  return 0;
}

```

```
$ gcc -ggdb test.c -o test.out
$ ./test.out
Floating point exception (core dumped)
```

###### 此处涉及到core dumped(内存转储)
Core的意思是内存, Dump的意思是扔出来, 堆出来.  
开发和使用Unix程序时, 有时程序莫名其妙的down了, 却没有任何的提示(有时候会提示core dumped). 这时候可以查看一下有没有形如core.进程号的文件生成, 这个文件便是操作系统把程序down掉时的内存内容扔出来生成的, 它可以做为调试程序的参考.  
core dump又叫核心转储, 当程序运行过程中发生异常, 程序异常退出时, 由操作系统把程序当前的内存状况存储在一个core文件中, 叫core dump.

## Backtracing

```
(gdb) bt
#0  0x000056468844813b in actual_calc (a=13, b=0) at test.c:3
#1  0x0000564688448171 in calc () at test.c:12
#2  0x000056468844818a in main () at test.c:17
```

## Frame inspection

```
(gdb) f 2
#2  0x000055fa2323318a in main () at test.c:17
17    calc();
(gdb) list
12    actual_calc(a, b);
13    return 0;
14  }
15  
16  int main(){
17    calc();
18    return 0;
19  }
(gdb) p a
No symbol "a" in current context.
```

