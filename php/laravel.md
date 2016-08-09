# laravel

## Issue

### TokenMismatchException

原因是没有设置域名

把`config/session.php`里的`'domain' => null`改为`'domain' => env('DOMAIN', null)`

## Base

### `session`持久化

`Session::save()`

无论是怎么修改session(set put flush)，之后不调用save的话都不会持久化，下次请求时还会还原

### `session`描述

laravel的session是独立自己重写的，不是用内置的。所以想和其它项目共用session，就必须自己在php.ini里全局设置或在代码里ini_set局部设置内置的session配置，而不是在laravel里设置，因为laravel的session设置只对laravel实现的session有效