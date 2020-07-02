# 基本

https://qiita.com/suke/items/770bccf8a43f9247daf5

# PEAR

```
{
    "require": {
        "php": "^7.2",
        "aws/aws-sdk-php": "^3.66",
        "phpoffice/phpspreadsheet": "^1.6",
        "pear-pear.php.net/PEAR" : "*",
        "pear-pear.php.net/File_Archive" : "*"
    },
    "config": {
        "process-timeout" : 0,
        "secure-http": false
    },
    "repositories": {
        "packagist": {
            "type": "composer",
            "url": "https://packagist.jp"
        },
        "pear": {
            "type": "pear",
            "url": "http://pear.php.net"
        }
    }
}
```