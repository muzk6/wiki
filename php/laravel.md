# laravel

## Issue

### TokenMismatchException

原因是没有设置域名

把`config/session.php`里的`'domain' => null`改为`'domain' => env('DOMAIN', null)`
并在`.env`里加上`DOMAIN=域名`