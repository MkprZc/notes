
json包里使用的时候，会结构体里的字段边上加tag，有没有什么办法可以获取到这个tag的内容呢？

```go

package main

import (
	"fmt"
	"reflect"
)

type J struct {
	a string //小写无tag
	b string `json:"B"` //小写+tag
	C string //大写无tag
	D string `json:"DD" otherTag:"good"` //大写+tag
}

func printTag(stru interface{}) {
	t := reflect.TypeOf(stru).Elem()
	for i := 0; i < t.NumField(); i++ {
		fmt.Printf("结构体内第%v个字段 %v 对应的json tag是 %v , 还有otherTag？ = %v \n", i+1, t.Field(i).Name, t.Field(i).Tag.Get("json"), t.Field(i).Tag.Get("otherTag"))
	}
}

func main() {
	j := J{
		a: "1",
		b: "2",
		C: "3",
		D: "4",
	}
	printTag(&j)
}

// 输出
/*

结构体内第1个字段 a 对应的json tag是  , 还有otherTag？ =
结构体内第2个字段 b 对应的json tag是 B , 还有otherTag？ =
结构体内第3个字段 C 对应的json tag是  , 还有otherTag？ =
结构体内第4个字段 D 对应的json tag是 DD , 还有otherTag？ = good


printTag方法传入的是j的指针。
reflect.TypeOf(stru).Elem()获取指针指向的值对应的结构体内容。
NumField()可以获得该结构体的含有几个字段。
遍历结构体内的字段，通过t.Field(i).Tag.Get("json")可以获取到tag为json的字段。
如果结构体的字段有多个tag，比如叫otherTag,同样可以通过t.Field(i).Tag.Get("otherTag")获得。


json包不能导出私有变量的tag是因为取不到反射信息的说法，但是直接取t.Field(i).Tag.Get("json")却可以获取到私有变量的json字段，是为什么呢？
其实准确的说法是，json包里不能导出私有变量的tag是因为json包里认为私有变量为不可导出的Unexported，所以跳过获取名为json的tag的内容。具体可以看/src/encoding/json/encode.go:1070的代码



json包里使用的时候，结构体里的变量不加tag能不能正常转成json里的字段？

    如果变量首字母小写，则为private。无论如何不能转，因为取不到反射信息。
    如果变量首字母大写，则为public。
        不加tag，可以正常转为json里的字段，json内字段名跟结构体内字段原名一致。
        加了tag，从struct转json的时候，json的字段名就是tag里的字段名，原字段名已经没用。

*/

```