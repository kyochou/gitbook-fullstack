# Install

## Script

### MacOS

```shell
brew install php@7.2
brew install composer
```

### Common

```shell
pecl install redis
pecl install xdebug

composer global require friendsofphp/php-cs-fixer
```

## MacOS
HomeBrew 自 2018-03-31 起弃用 `homebrew/php`，php 版本改名(如: php70 => php@7.0), 扩展可采用 pecl 安装.

```shell
brew install php@7.2
```

如果安装后运行 `php --ini` 命令提示 `Unable to load dynamic library`, 则需要在 php.ini 中将 `extension_dir` 的值指向 pecl 库的地址: `extension_dir = "/usr/local/lib/php/pecl/20160303"`.

```shell
# brew install homebrew/php/php71
# brew install php71-intl
# PHP Data Structures
# brew install homebrew/php/php71-ds
# php 代码格式检查和格式化
# composer global require "squizlabs/php_codesniffer=*"
# composer global require friendsofphp/php-cs-fixer
```

#### 安装多个 PHP 版本

```
brew install php-version
# 临时切换
source $(brew --prefix php-version)/php-version.sh && php-version 5
# 永久切换
brew unlink php56
brew link php71
```

#### 参考
[Mac 下 php 版本切换](https://code-ken.github.io/2016/02/16/mac-php-version/)

## Extensions
### XDebug
```shell
pecl install xdebug
```
调试 CLI 程序时, 使用入口文件做为调试的开始. 如 `php think Platserver` 这个命令, 需要调试的文件为 think, 然后在 Platserver 中设置断点即可.

## Tools
### php-cs-fixer
[PHP Coding Standards Fixer](https://cs.symfony.com/): 代码格式化工具.

```shell
composer global require friendsofphp/php-cs-fixer
```

## Resources
* [PHP The Right Way](https://phptherightway.com/)
* [Modern PHP（中文版）](https://about.ac/books/modern-php/)