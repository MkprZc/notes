
> An awesome project.

golang mysql 死锁解决 linux

## Go

数据类型的转换
字符串到 整型，之类的
整型 到 字符串


字符串的高性能拼接

字符串常用的 包 strings


go 的list 包


函数的参数是值传递，go中都是值传递

函数的可变参数： ...

函数闭包

func aa() func() int {
	local := 0
	return func() int {
		local += 1
		return local
	}
}


defer 会在return 之前执行

按照指定顺序遍历map


指针的内存图未画

switch 

排序算法（算法库）

引用类型 map、channel 是指针包装，range 复制值是什么
