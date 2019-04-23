### 命名规范

#### 1、名字很重要，这个讲话是关于Go的命名。

- 可读性是良好代码的定义质量。
- 好名字对可读性至关重要。

#### 2、好的命名

- 短  (容易打字)  
- 一致(易于猜测) 
- 准确(易于理解)

#### 3、经验法则

    名称声明与其使用之间的距离越大， 名称应该越长。


#### 4、使用MixedCase
    Go中的名称应该使用MixedCase。(不要使用names_with_underscores。)，如ServeHTTP和IDProcessor。

#### 5、局部变量

- 保持简短; 长名称模糊了代码的作用。
- 常见的变量/类型组合可能使用非常短的名称：  
   
        Prefer i to index. 
        Prefer r to reader. 
        Prefer b to buffer.

- 根据上下文，避免使用冗余名称：

    - 宁可在名字为runecount的函数内使用count，也不要在函数内使用runecount。
    - 与在语句中，首选OK  
    
```gotemplate
//较长的名称可能有助于长函数或具有许多局部变量的函数。 (但这通常意味着你应该重构。)
 v，ok：= m [k]
```
  

demo
```gotemplate

// Bad
func RuneCount(buffer []byte) int {
    runeCount := 0
    for index := 0; index < len(buffer); {
        if buffer[index] < RuneSelf {
            index++
        } else {
            _, size := DecodeRune(buffer[index:])
            index += size
        }
        runeCount++
    }
    return runeCount
}


//good 
func RuneCount(b []byte) int {
    count := 0
    for i := 0; i < len(b); {
        if b[i] < RuneSelf {
            i++
        } else {
            _, n := DecodeRune(b[i:])
            i += n
        }
        count++
    }
    return count
}
```    

#### 6、参数

函数参数类似于局部变量， 但它们也可用作文档。  

- 如果类型是描述性的，它们应该简短：

```gotemplate
func AfterFunc(d Duration, f func()) *Timer

func Escape(w io.Writer, s []byte)

```
- 如果类型更模糊，名称可能提供文档：

```gotemplate
func Unix(sec, nsec int64) Time

func HasPrefix(s, prefix []byte) bool
```

#### 7、返回值

仅应出于文档目的命名导出函数的返回值。

命名返回值的好例子：
```gotemplate
func Copy(dst Writer, src Reader) (written int64, err error)

func ScanBytes(data []byte, atEOF bool) (advance int, token []byte, err error)

```

#### 8、

接收者是一种特殊的论点。

按照惯例，它们是反映接收器类型的一个或两个字符，
因为它们通常出现在几乎每一行上：
```gotemplate

func (b *Buffer) Read(p []byte) (n int, err error)

func (sh serverHandler) ServeHTTP(rw ResponseWriter, req *Request)

func (r Rectangle) Size() Point
```

接收者名称在类型的方法中应该是一致的。(不要r在一种方法和rdr另一种方法中使用。)


#### 9、导出的包级别名称
     
导出的名称由其包名限定。
     
在命名导出的变量，常量，函数和类型时请记住这一点。

这就是为什么我们有:
- bytes.Buffer和strings.Reader
- bytes.ByteBuffer和strings.StringReader。

#### 10、接口类型

仅指定一个方法的接口通常只是附加了“er”的函数名称。

```gotemplate
type Reader interface {
    Read(p []byte) (n int, err error)
}
```

有时结果不是正确的英语，但我们仍然这样做：

```gotemplate
type Execer interface {
    Exec(query string, args []Value) (Result, error)
}
```

有时我们使用英语使其更好：

```gotemplate
type ByteReader interface {
    ReadByte() (c byte, err error)
}
```
当接口包括多个方法中，选择准确地描述其用途的名称
（例如：net.Conn，http.ResponseWriter，io.ReadWriter）。

#### 11、错误类型

应为以下形式FooError：
```gotemplate
type ExitError struct {
    ...
}
```

错误值应为以下形式ErrFoo：
```gotemplate
var ErrFormat = errors.New("image: unknown format")

```

#### 12、包
选择对导出的名称有意义的包名称。
避开util，common等。


#### 13、导入路径

包路径的最后一个组件应与包名相同。
```gotemplate
"compress/gzip" // package gzip
```

避免在存储库和包路径中断断续续：
```gotemplate
"code.google.com/p/goauth2/oauth2" // bad; my fault
```

对于库，它通常用于将包代码放在repo根目录中：
```gotemplate
"github.com/golang/oauth2" // package oauth2
```
还要避免使用大写字母（并非所有文件系统都区分大小写）。

