# config

## xdebug

* 基本配置

```
[xdebug]
zend_extension="D:/xampp/php/ext/php_xdebug.dll"
xdebug.remote_autostart=on
xdebug.remote_enable=on
xdebug.remote_mode="req"
;xdebug.remote_log="/var/log/xdebug.log"
xdebug.remote_host=127.0.0.1
xdebug.remote_port=9000
xdebug.remote_handler="dbgp"
xdebug.idekey="PhpStorm"
```

* 远程调试服务器的设置

> [参考](https://confluence.jetbrains.com/display/PhpStorm/Remote+debugging+in+PhpStorm+via+SSH+tunnel)

必须用到SSH通道，在windows调试远程服务器时只需参考上面官方文档的`Setting up an SSH tunnel on Windows`，不要看`Setting up an SSH tunnel on Linux or Mac OS X`
在putty的Session添加完账号后，接着在Connection -> SSH -> Tunnels按文档说明来添加，
之后打开终端登录远程服务器，就会自动创建SSH通道。其远程调试的原理就是通过SSH通道来
调试的。