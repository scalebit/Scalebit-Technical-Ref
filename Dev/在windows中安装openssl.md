在windows当中，rust项目在进行`cargo build`的过程中，非常容易产生以下错误：

`failed to run custom build command for openssl-sys`

这是由于在windows当中安装openssl兼容性较差导致的，我认为最佳实践可以参考以下链接

https://github.com/sfackler/rust-openssl/issues/1086

但是于此同时，还需要额外增加一些env的设置才能够顺利编译：

`$env:OPENSSL_INCLUDE_DIR = "C:\vcpkg\installed\x64-windows-static\include"`
`$env:OPENSSL_LIB_DIR  = "C:\vcpkg\installed\x64-windows-static\lib"`

在mac和linux中几乎不会出现这个问题

每次在运行控制台的时候，都需要重新键入所有带有$env的命令。当然你可以采用环境变量解决这个问题；很意外的是，我的机器上使用环境变量是无效的。