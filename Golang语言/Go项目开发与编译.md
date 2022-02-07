# Go项目开发与编译

[Go 模块解惑：到处都是 GO111MODULE ，这到底什么？](https://learnku.com/go/t/39086)

1. Go模块包依赖管理

    Go 1.11新增了对模块的支持，希望能够解决“包依赖管理”。可以通过设置环境变GO111MODULE来开启或关闭模块支持，它有三个可选值：off，on，auto，默认值是auto

    - GO111MODULE = off

        无模块支持，去会从GOPATH和vendor文件夹寻找包。

    - GO111MODULE = on

        模块支持，go会忽略GOPATH和vendor文件夹，只根据go.mod下载依赖。

    - GO111MODULE = auto

        默认模式。在这种模式下，Go 会表现

        1.11，1.12

        - 当您在 `GOPATH` 外部时， 设置为 `GO111MODULE = on`
        - 当您位于 `GOPATH` 内部时，即使存在 `go.mod`, 设置为 `GO111MODULE = off`

        1.13

        - 当存在 `go.mod` 文件时或处于 GOPATH 外， 其行为均会等同于于 `GO111MODULE=on`
        - 当处于 GOPATH 内且没有 `go.mod` 文件存在时其行为会等同于 `GO111MODULE=off`

GOPATH内部使用go mod init 需要指定module

1. Go程序的编译

    在Go语言中，和编译有关的命令主要是go run ,go build , go install这三个命令。

    - go run只能作用于main包文件，先运行compile 命令编译生成.a文件，然后 link 生成最终可执行文件并运行程序，这过程的产生的是临时文件，在go run 退出前会删除这些临时文件（含.a文件和可执行文件）。最后直接在命令行输出程序执行结果。go run 命令在第二次执行的时候，如果发现导入的代码包没有发生变化，那么 go run 不会再次编译这个导入的代码包，直接进行链接生成最终可执行文件并运行程序。
    - go install用于编译并安装指定的代码包及它们的依赖包，并且将编译后生成的可执行文件放到 bin 目录下（GOPATH/bin），编译后的包文件放到当前工作区的 pkg 的平台相关目录下
    - go build用于编译指定的代码包以及它们的依赖包。如果用来编译非main包的源码，则只做检查性的编译，而不会输出任何结果文件。如果是一个可执行程序的源码（即是 main 包），这个过程与go run 大体相同，除了会在当前目录生成一个可执行文件外。使用go build时有一个地方需要注意，对外发布编译文件如果不希望被人看到源代码，请使用go build -ldflags 命令，设置编译参数-ldflags "-w -s" 再编译后发布。避免使用gdb来调试而清楚看到源代码。

    两个重要的环境变量GOOS和GOARCH,

    其中GOOS指的是目标操作系统，

    它的可用值为：

    ```c
    1. darwin
    2. freebsd
    3. linux
    4. windows
    5. android
    6. dragonfly
    7. netbsd
    8. openbsd
    9. plan9
    10. solaris
    ```

    一共支持10中操作系统。GOARCH指的是目标处理器的架构，

    目前支持的有：

    ```c
    1. arm
    2. arm64
    3. 386
    4. amd64
    5. ppc64
    6. ppc64le
    7. mips64
    8. mips64le
    9. s390x
    ```

    一共支持9中处理器的架构，GOOS和GOARCH组合起来，支持生成的可执行程序种类很多，具体组合参考[https://golang.org/doc/install/source#environment](https://golang.org/doc/install/source#environment)。

    ```c
    # Mac 下编译 Linux 和 Windows 64位可执行程序
    CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build 
    CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build 

    CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build 

    # linux 下编译 Mac 和 Windows 64位可执行程序
    CGO_ENABLED=0 GOOS=darwin GOARCH=amd64 go build 
    CGO_ENABLED=0 GOOS=windows GOARCH=amd64 go build 

    # Windows 下编译 Mac 和 Linux 64位可执行程序
    SET CGO_ENABLED=0SET GOOS=darwinSET GOARCH=amd64 go build main.go
    SET CGO_ENABLED=0SET GOOS=linuxSET GOARCH=amd64 go build main.go
    ```