# OC对象本质(一)

> 一个NSObject对象占用多少内存？

```
NSObject * obj = [[NSObject alloc]init];
```
可以看做是再问obj指针指向的内存区域有多大

## Object-C的本质
Object-C底层实现其实都是C/C++代码，所以Object-C的面向对象都是基于C/C++的结构体实现的。

```
Object-C -> C -> C++ -> 汇编语言 -> 机器语言
```

将Object-C转化为C/C++

```
xcrun -sdk iphoneos clang -arch arm64 -rewrite-objc main.m -o main.cpp
```

1. iphoneos：平台
2. arm64 : arm32/arm64
3. main.m ：需要转化的文件名
4. main.cpp:输出文件名
5. Nobject内存本质


创建Demo工程，修改main.m文件,创建NSObject对象，然后前往main.m文件所在目录，执行转换命令

```

int main(int argc, char * argv[]) {
    @autoreleasepool {
       NSObject * obj = [[NSObject alloc]init];        
        return UIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegate class]));
    }
}
```

执行完成后生成main.cpp文件，打开main.cpp文件，搜索NSObject,会看到如下代码：

```
代码1

struct NSObject_IMPL {
    Class isa;
};
```

这个结构体就是NSObject对象在内存中的本质

```
代码2

struct NSEnumerator_IMPL {
    struct NSObject_IMPL NSObject_IVARS;
};
```
这和C++源码中的结构体极为相似，也证实了NSObject对象的本质就是一个C++结构体。