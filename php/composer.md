# composer

## 初始化
`$ composer init`

---

## psr-4 配置
* composer.json
* 命名空间 \Aa\Bb\Cc 对应目录 src/lib/
* 命名空间 \Aa\Bb\Cc\Dd 对应目录 src/lib/ExternalApi/
* 库目录结构为 doc/ => 文档目录, src/lib/ => 类库目录, src/conf/ => 配置文件目录
```
{
    "autoload": {
        "psr-4": {
            "Aa\\Bb\\Cc\\": "src/lib/",
            "Aa\\Bb\\Cc\\Dd\\": "src/lib/ExternalApi/"
        }
    }
}
```

## 更新 vendor 目录
* 安全更新目录，不会去更新下载依赖
* 例如修改了配置的 psr-4 ，就会按照配置去更新命名空间

`composer update --lock`