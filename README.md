# Fuchsia
目前仅支持在Intel NUC上安装FuchsiaOS

## 准备
你需要先安装最新版本的Xcode,再在终端运行:
```shell
xcode-select --install
```
并且请确保你能够顺畅地访问
- fuchsia.googlesource.com
- chrome-infra-packages.appspot.com

## 获取
Fuchsia采用了`jiri`作为源代码管理工具.Google提供了一份自动化脚本以下载源码:
```shell
curl -s "https://fuchsia.googlesource.com/fuchsia/+/master/scripts/bootstrap?format=TEXT" | base64 --decode | bash
```
这段代码会在当前目录下新建一个名为`fuchsia`的目录,然后将预编译的`jiri`与`fx`脚本放入该目录中的`.jiri_root/bin`目录下,并将完整的代码置入`fuchsia`目录中.

## 编译
Fuchsia的编译参数格式如下:
```shell
fx set <product>.<board> [--release]
```
`product`包含四个选项,分别是`core`(仅内核),`Terminal`(含终端),`Workstatiom`(含GUI)与`router`(路由).
请使用`fx list-boards`以检查你所使用的NUC的相应版本的`board`.
之后您只需要执行:
```shell
fx build
```

## 导出
你需要使用以下命令制作一个启动盘:
```shell
fx mkzedboot <udisk_device>
```
请将制作好的启动盘链接至NUC,并且以其引导启动.
在NUC上的`zedboot`会通过`mdns协议`对外广播自己的存在.

## 安装
请在终端中运行以下代码以完成分区与安装:
```shell
install-disk-image wipe
dm reboot
fx pave
```
期间NUC可能会重启一次.

## Enjoy it!
