# config

## 开放文件访问权限

```
Alias /dev "D:/Documents/php"
<Directory "D:/Documents/php">
    Options Indexes FollowSymLinks MultiViews
    AllowOverride all
    Order Deny,Allow
    Allow from all
    Require all granted
</Directory>
```