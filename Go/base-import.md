# 诡异:区别于其他语言

## 一 `:`

### 1. 声明变量并赋值

```
i := true // :左边的值，必需有未声明的变量，允许有已经声明的变量。
```

### 2. 切片

__返回slice的一部分,共用了内存。__


```
s := []int{1,2,3,4}

s[:] // 1,2,3,4
s[1:] // 2,3,4
s[:2] // 1,2
s[1:2] // 2
```

## 二 `...`

### 1. 传递不定个数参数时，用`...`, 形参是个切片。

```
func buy(some ...string) bool {
	for i,it := range some {
		fmt.Println(i,it)
	}
	return true
}
```

### 2. 可以把切片转化为需要的实参（一个一个值）。

```
some := []string{"春联","玩具","红包","糖果","衣服"}
buy("春联","玩具","红包","糖果","衣服")
buy(some...)
buy(some[1:]...)
```

## 三 `_`

### `_`表示忽略变量、包

```
ok := buy("车票1")
_ = buy("车票2") // 表示忽略返回结果

_,err := funcA() // 忽略第一个返回值

import _ "time"
```

## 四 `.`

### import 时，`.` 可以像本包方法一样使用,而不需要带包名就可以调用引用包的方法。

```
import (
	"fmt"
	. "strings"
	_ "log"
)

import _ "time"
```

go里的规则：__引用必需使用，除非忽略引用。__

代码里忽略引用的`log`,`time`，可以不调用（因为被忽略，已无法显示调用）；
`strings` 引用了，必需调用任何一个方法或引用任何一个变量。

