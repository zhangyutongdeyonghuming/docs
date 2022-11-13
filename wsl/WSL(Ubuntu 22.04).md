## wsl安装rust

> 打开官网可以看到, 官方给出了脚本, 直接复制执行即可

![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211132133900.png)

```shell
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

> 我已经安装过了, 所以打印出了我的rust信息, 未安装的直接选择圈中的默认安装(回车直接执行)即可.

![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211132135159.png)

> 看到rustc/cargo都能打印就是安装好了

### 新建项目

```shell
cargo new hello
```

### 项目启动
```shell
cargo r
```

> 我在此处碰到了错误/usr/bin/ld: cannot find crti.o: No such file or directory

### 错误处理

#### 执行编译时error: linking with `cc` failed: exit code: 1

> ......
> [ /usr/bin/ld: cannot find crti.o: No such file or directory]

> 该错误是因为缺少libc6-dev

```shell
sudo apt install -y libc6-dev
```

> 执行时可能发生依赖版本太高/低的的错误, 检查自己的`source.list`源文件和自己的系统版本是否能对上

![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211132140768.png)

> `codename`要和源的`codename`对应, 否则系统依赖会出问题, 修改完`source.list`之后

```shell
sudo apt update && sudo apt upgrade
```

> 自动安装好了libc6-dev, 大功告成, `hello world`!
![](https://raw.githubusercontent.com/zhangyutongdeyonghuming/images/main/202211132142447.png)
