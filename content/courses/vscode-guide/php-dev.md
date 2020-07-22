### 插件推荐

- [PHP DocBlocker](https://marketplace.visualstudio.com/items?itemName=neilbrayfield.php-docblocker)：PHP 注释插件，在 PHP 代码中输入 `/**` 快速生成注释。
- [PHP Intelephense](https://marketplace.visualstudio.com/items?itemName=bmewburn.vscode-intelephense-client)：PHP 智能提示插件，可以完成代码提示、上下文跳转、代码格式化等功能。
- [php cs fixer](https://marketplace.visualstudio.com/items?itemName=junstyle.php-cs-fixer)：PHP 代码格式化专用插件。
- [phpcs](https://marketplace.visualstudio.com/items?itemName=ikappas.phpcs)：PHP 代码格式校验专用插件，此插件依赖 php-cs-fixer 命令行工具，在 macOS 中可以通过 `brew install php-cs-fixer` 进行安装。
- [MySQL](https://marketplace.visualstudio.com/items?itemName=cweijan.vscode-mysql-manager)：MySQL 管理插件。
- [PHP Debug](https://marketplace.visualstudio.com/items?itemName=felixfbecker.php-debug)：PHP 断点调试插件。

### 相关配置

```json
{
  "php.validate.executablePath": "/usr/local/bin/php", // PHP 路径
  "php.suggest.basic": false // 防止默认提示与 Intelephense 插件提示冲突
}
```
