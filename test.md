# Go map
* map 的声明方式：

```go
m := map[string]string {
    "name" : "Monkey",
    "age" : "20",
    "class" : "Golang"
}
```

* 创建 map ：
    * `map[K]V`

    * 创建 map. ： `make(map[string]int)` 

    * 可以声明复合 `map[K1]map[K2]V2 `
* 获取元素
    * `m[key]`
* key 不存在时，获得的 value 类型的初始值
* 用 `value, ok := m[key]` 来判断是否存在 key
* 删除 一个 key : `delete(loopUp, "name")`: 删除 loopUp 中的 key 为 name 的元素

* map 的遍历：
    * 使用 `range` 遍历 `key` , 或者遍历 `key, value 对` ；
    * 不保证遍历顺序， 如需顺序，需要手动对 key. 排序；
        * 可以把遍历的 map 放到 slice 中，然后对 slice 进行排序。最后把 slice 加到 map 中，此时的 map 就是排好序的；

* 使用 `len` 来获得元素的个数；

* **map 的 key**
    * map 使用哈希表，必须可以比较相等
    * 除了 `slice`， `map`, `function` 的内建类型都可以作为 key;
    * `struct` 类型不包含上述字段，也可以作为 key; 

```go
package main

import (
	"fmt"
)

func getMapsVal() {
	looUp := map[string]string {
		"goku": "goku-val",
		"name" : "monkey",
		"classes" : "php",
	}
	// key value 是无序的，hashmap
	// 当获取 map 中不存在的 value 时，会出现 零值，空字符串
	// 变量不初始化也可以用，只不过是零值
	for index, value := range looUp  {
		fmt.Print(index, " value is " + value + "\n");
		/**
			name value is monkey
			classes value is php
			goku value is goku-val
		 */
	}
	//age := looUp["age"]
	//fmt.Println(age) // ""
	// 判断 key 是否存在, 在获取key时，使用第二个参数[exists bool ]接收，即可判断该key是否存在。
	//orderNum, exists := looUp["order_num"]
	//fmt.Println(orderNum, exists) // "" false
	if orderNum, ok := looUp["order_num"]; ok { // 可以使用 if 判断 该 key 是否存在
		fmt.Println(orderNum)
		fmt.Println("123")
	} else {
		fmt.Println("key does not exist")
	}
	// 删除 map 中的某个元素, 使用 delete
	delete(looUp, "name")
	fmt.Println(looUp) // map[classes:php goku:goku-val]


}


func main() {
	getMapsVal()
}

```

* map 的示例：

> 寻找最长不含有重复字符的字串
> abcabcbb ---> abc 
> bbbbbb ---> b
> pwwkew ---> wke

![](media/15567701412024/15571491342683.jpg)


```go
// todo 寻找最长不含有重复字符的字串
```

* Go 字符串的处理 `string`, `runne` 
    
    ```go
    package main

    import (
    	"fmt"
    	"unicode/utf8"
    )
    
    func main() {
    	s := "Yes人民警察爱人民"
    	fmt.Println(len(s)) // 24
    	fmt.Printf("%s\n", s) // Yes人民警察爱人民
    	for _, b := range []byte(s) {
    		// ASCII码[每个中文三字节] 59 65 73 E4 BA BA【人】 E6 B0 91【民】 E8 AD A6【】 E5 AF 9F【】 E7 88 B1【】 E4 BA BA【】 E6 B0 91【】
    		fmt.Printf("%X ", b)
    	}
    	fmt.Printf("\n")
    	for i, ch := range s {
    		// 下标是字节
    		// 每个中文三个字节 (0, 59)(1, 65)(2, 73)(3, 4EBA)【人】(6, 6C11)【民】(9, 8B66)【警】(12, 5BDF)【察】(15, 7231)【爱】(18, 4EBA)【人】(21, 6C11)【民】
    		fmt.Printf("(%d, %X)", i, ch)
    	}
    	fmt.Printf("\n")
    	fmt.Println(utf8.RuneCountInString(s)) // 10
    	fmt.Printf("\n")
    	bytes := []byte(s) // byte slice
    
    	for len(bytes) > 0 {
    		ch, size := utf8.DecodeRune(bytes)
    		bytes = bytes[size:]
    		fmt.Printf("%c ", ch) // Y e s 人 民 警 察 爱 人 民
    	}
    	fmt.Printf("\n")
    	//fmt.Printf("%X\n", []byte(s)) // 596573E4BABAE6B091E8ADA6E5AF9FE788B1E4BABAE6B091
    
    	// rune 的底层是，重新开辟一块内存，把 runne slice 保存下来
    	for i, ch := range []rune(s) {
    		//(0, Y) (1, e) (2, s) (3, 人) (4, 民) (5, 警) (6, 察) (7, 爱) (8, 人) (9, 民)
    		fmt.Printf("(%d, %c) ", i, ch)
    
    	}
    }

    ```

    * 处理中文时使用 rune;
    * 使用 `range` 遍历 `pos`, `rune` 对；
    * 使用 `urf8.RuneCountInString` 获得字符数量；
    * 使用 `len` 获得字节的长度
    * 使用 `[]byte` 获得字节；
    * 使用 `[]rune` 会进行所有的转换，把所有转换后的结果放到一个数组里面，再开一个 `rune slice` 返回

    ```go
    // todo 寻找最长不含有重复字符的字串 包含中文
    ```
    
* 字符串操作
    * Fields
        * `Fields` :去除字符串s中的空格符, 并按照空格(可以是一个或者多个空格)分割字符串, 返回slice
        * `func Fields(s string) []string`
    * Split
        * `Split` : 把字符串按照sep进行分割, 返回slice
        * `func Split(s, sep string) []string`
    * Join
        * `Join` : 字符串拼接, 把slice通过给定的sep连接成一个字符串
        * `func Join(a []string, sep string) string`
    * Conteains, Index
        * `Conteains`: 判断给定字符串s中是否包含子串substr, 找到返回true, 找不到返回false
        * `Index`: 在字符串s中查找sep所在的位置, 返回位置值, 找不到返回-1
    * ToLower, ToUpper
        * `ToLower` : 所有字母转换为小写
        * `ToUpper` : 所有字母转换为大写
    * Trim, TrimRight, TrimLest
        *  `Trim` : 删除在s字符串的头部和尾部中由cutset指定的字符, 并返回删除后的字符串
        *  `func Trim(s string, cutset string) string`
        *  `TrimRight` :  删除在s字符串的尾部中由cutset指定的字符, 并返回删除后的字符串
        *  `TrimeLeft` :  删除在s字符串的头部中由cutset指定的字符, 并返回删除后的字符串