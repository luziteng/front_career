## Golang 语言的官网

首先我们登录 Golang 的[官方网站](https://golang.org/),当然你也可以登录 Golang 的[国内网站](https://golang.google.cn/)。
在 Mac、Windows 和 Linux三个平台上都支持 Golang。
您可以您可以从[下载](https://golang.org/dl/)相应平台的安装包。该网站在国内不容易访问，所以可以访问[中国对应的网站](https://golang.google.cn/dl/)或者是 [Go 语言中文网](https://www.studygolang.com/dl) 进行安装软件的下载。

![下载事例](../../assets/downlads.png)

### Mac 安装配置

1. **Golang 官网，进入下载页面，选择对应自己操作系统的版本下载安装。我是使用Homebrew 直接使用`brew install go` 来安装。安装成功后，在命令行输入 `go env`或者查看Go 版本`go version` 验证是否成功**。
咱们`cd /usr/local/go` 会看到安装Go生成的文件。

2. **配置环境变量**

安装成功后一般不需要配置环境变量，如果需要可以按一下方式进行配置。

命令输入 `open -e .bash_plofile` 编辑输入

```
export GOROOT=/usr/local/go
export GOPATH=/users/lupeng/go   // 注意：这里的lupeng 需要置换成你的文件名
export GOBIN=$GOROOT/bin
export PATH=$PATH:$GOBIN
```

![编辑go](../../assets/editgo.png)
保持后，输入`source ~/.bash_profile`文件，使之生效

:warning::warning: 如果未找到.bash_plofile文件，可通过命令`touch .bash_profile`创建，然后进行编辑

### 触摸Go语言

#### 第一个hello world 程序

学习一门语言，第一个程序从hello world 开始，咱们也不能免俗。搞出hello world,咱们就算触碰到Go这门语言啦:stuck_out_tongue_winking_eye:

1. 创建包含`.go`后缀的文件，这里我命名为`helloworld.go`
2. 在文件中输入如下代码：

```
package main
import "fmt"
func main () {
/* 这是第⼀个简单的程序  */
fmt.Println ("Hello, World!")
}
```

3. 在终端输入 `go run helloworld.go`,在会终端打印出来 `Hello, World!`，那说明咱们成功啦！

#### 程序解释

1. 第⼀⾏代码 package main 定义了包名。必须在源⽂件中⾮注释的第⼀⾏ 指明这个⽂件属于哪个包，如：`package main` 。`package main`表示⼀个可独⽴执⾏的程序，每个 Go 应⽤程序都包含⼀个名为 main 包。
2. 下⼀⾏ `import "fmt"` 告诉 Go 编译器这个程序需要使⽤ fmt 包，  fmt 包实 现了格式化 IO  （输⼊/输出）的函数。
3. 下⼀⾏ `func main()` 是程序⼊⼝。  main 函数是每⼀个可执⾏程序所必须包含的，⼀般来说都是在启动后第⼀个执⾏的函数，如果有 init() 函数则会先执⾏init()函数。
4. 下⼀⾏` /*...*/ `是注释，在程序执⾏时将被忽略。单⾏注释是最常⻅的注释形式，你可以在任何地⽅使⽤以 `//`开头的单⾏注释。多⾏注释也叫块注释，
均已以``/*开头，并以*/``结尾，且不可以嵌套使⽤，多⾏注释⼀般⽤于⽂档 描述或代码⽚段。
5. 下⼀⾏ ``fmt.Println(...)`` 可以将字符串输出到控制台，并在最后⾃动增加换 ⾏字符 `\n`。使⽤ `fmt.Print("hello, world\n")`可以得到相同的结果。

#### Go语⾔编码规范

- **注释**
  单⾏注释是最常⻅的注释形式，你可以在任何地⽅使⽤以 `//` 开头的单⾏注释
  多⾏注释也叫块注释，均已以 `/*开头，并以 */` 结尾，且不可以嵌套使⽤，多⾏注释⼀般⽤于⽂档描述或注释成块的代码⽚段
- **标识符**
  1. 标识符⽤来命名变量、类型等程序实体。⼀个标识符实际上就是⼀个或是多个字⺟(A~Z和a~z)数字(0~9)、下划线_组成的序列，但是第⼀个字符必须是字⺟或下划线⽽不能是数字。
  2. Go不允许在标识符中使⽤@、$和%等标点符号。
  3. Go是⼀种区分⼤⼩写的编程语⾔。因此，Manpower和manpower是两个不 同的标识符。
  4. 以下是⽆效的标识符：

  - 1xy  （以数字开头）
  - case  （Go 语⾔的关键字）
  - chan  （Go 语⾔的关键字）
  - x+y  （运算符是不允许的）

- **Go语⾔的空格**
  
1. Go 语⾔中变量的声明必须使⽤空格隔开，如：  var age int;
2. 语句中适当使⽤空格能让程序更易阅读。
3. 在变量与运算符间加⼊空格，程序看起来更加美观，如：   a = x + y;

- **语句的结尾**

1. 在 Go 程序中，⼀⾏代表⼀个语句结束。Go语⾔中是不需要类似于Java需要分号结尾，因为这些⼯作都将由 Go 编译器⾃动完成；
2. 如果打算将多个语句写在同⼀⾏，它们则必须使⽤ 分号“;”⼈为区分，但在 实际开发中并不⿎励这种做法。

- **可⻅性规则**  
  
1. Go语⾔中，使⽤⼤⼩写来决定标识符（常量、变量、类型、接⼝、结构或函 数）是否可以被外部包所调⽤。
2. 以⼀个⼤写字⺟开头，那么使⽤这种形式的标识符的对象就可以被外部包的代码所使⽤（使⽤时程序需要先导⼊这个包），如同⾯向对象语⾔中的 `public`。
3. 如果以⼩写字⺟开头，则对包外是不可⻅的，但是他们在整个包的内部是可⻅并且可⽤的，像⾯向对象语⾔中的`private` 。

#### Go语⾔关键字及保留字

下⾯列举了 Go 代码中会使⽤到的 25 个关键字或保留字：

| break | default | func | interface | select |
| :---- | :------ | :--- | :-------- | :----- |
| case  | defer | go | map | struct |
| chan  | else | goto | package | switch |
| const | fallthrough | if | range | type |
| continue | for | import | return | var |

除了以上介绍的这些关键字，  Go 语⾔还有 36 个**预定义标识符**：

| append | bool | byte | cap | close | complex | complex64 |
| :----- | :--- | :--- | :-- | :---- | :------ | :-------- |
| copy | false | float32 | float64 | imag | int | int8 |
| int32 |  int64 | iota | len | make | new | nil |
| print | println | real | recover | string | true | uint|

#### Go 程序结构组成

1. **Go⼀般结构**

- package main// 当前程序的包名
- import . "fmt"// 导⼊其他包
- const PI = 3.14// 常量定义
- var name = "gopher"// 全局变量的声明和赋值
- type newType int// ⼀般类型声明
- type gopher struct{}// 结构的声明
- type golang interface{}// 接⼝的声明
- func main () { Println ("Hello World!")}// 由main函数作为程序⼊⼝点启动

2. **Go ⽂件的基本组成部分**
   
- 包声明
- 引⼊包
- 函数
- 变量
- 语句 & 表达式
- 注释

#### Go⽂件结构组成

- Go 程序是通过 package 来组织的。
  只有 package 名称为 main 的包可以包含 main 函数。
  ⼀个可执⾏程序有且仅有⼀个 main 包。
  通过 import 关键字来导⼊其他⾮ main 包。
  可以通过 import 关键字单个导⼊，也可以同时导⼊多个
- 程序⼀般由关键字、常量、变量、运算符、类型和函数组成。
- 程序中可能会使⽤到这些分隔符：括号 ()，中括号 [] 和⼤括号 {}。
- 程序中可能会使⽤到这些标点符号：
  点.
  逗号,
  分号;
  冒号:
  省略号(三个点) …
- 通过 const 关键字来进⾏常量的定义。
- 通过在函数体外部使⽤ var 关键字来进⾏全局变量的声明和赋值。
- 通过 type 关键字来进⾏结构(struct)和接⼝(interface)的声明。
- 通过 func 关键字来进⾏函数的声明。

### 起风了 :boat::boat

*:tractor:最后请允许我分享好物，买辣椒也用卷的旧版**起风了**,因为实在太爱这个版本了所以从网易扒了下来，大家听听就好，我知道大家有办法拿到视频源，:lock:**请勿传播**:lock:，听听就好！！*

<video height=498 width='100%' src="../../assets/video/原版起风了.mp4" controls="controls"></video>
