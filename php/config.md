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

需要用到SSH通道，参考上面官方文档的`Setting up an SSH tunnel on Windows`，不要看`Setting up an SSH tunnel on Linux or Mac OS X`