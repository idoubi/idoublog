## 配置参数详解

```json
{
  "editor.fontSize": 14, // 字体大小
  "editor.formatOnSave": true, // 保存时自动格式化
  "files.autoSave": "onFocusChange", // 失去焦点时自动保存文件
  "breadcrumbs.enabled": true, // 显示文件路径
  "window.zoomLevel": 0, // 窗口缩放比例
  "workbench.statusBar.visible": true, // 隐藏底部状态栏
  "workbench.colorTheme": "An Old Hope Italic", // 编辑器主题
  "workbench.iconTheme": "eq-material-theme-icons-ocean", // 图标主题
  "workbench.startupEditor": "newUntitledFile", // 编辑器欢迎设置
  "explorer.confirmDragAndDrop": false, // 移动文件时是否需要确认
  "explorer.confirmDelete": false, // 删除文件时是否需要确认
  // 优化右侧预览地图样式
  "editor.minimap.renderCharacters": false,
  "editor.minimap.maxColumn": 200,
  "editor.minimap.showSlider": "always",
  // go开发配置
  "go.buildOnSave": "workspace",
  "go.lintOnSave": "package",
  "go.vetOnSave": "package",
  "go.buildFlags": [],
  "go.lintFlags": [],
  "go.vetFlags": [],
  "go.coverOnSave": false,
  "go.autocompleteUnimportedPackages": true,
  "go.useLanguageServer": true,
  "go.inferGopath": true,
  "go.docsTool": "godoc",
  "go.gocodePackageLookupMode": "go",
  "go.gotoSymbol.includeImports": true,
  "go.useCodeSnippetsOnFunctionSuggest": true,
  "go.useCodeSnippetsOnFunctionSuggestWithoutType": true,
  "go.formatTool": "goreturns",
  "go.gocodeAutoBuild": false,
  "go.liveErrors": {
    "enabled": true,
    "delay": 0
  },
  "go.gopath": "/data/go",
  "go.goroot": "/usr/local/Cellar/go/1.12.7/libexec",
  // php开发配置
  "php.validate.executablePath": "/usr/local/bin/php",
  "php.suggest.basic": false,
  "php-cs-fixer.executablePath": "/usr/local/bin/php-cs-fixer",
  // git配置
  "git.autofetch": true,
  "gitlens.advanced.messages": {
    "suppressFileNotUnderSourceControlWarning": true
  },
  "diffEditor.ignoreTrimWhitespace": true,
  // vscode-fileheader插件配置
  "fileheader.Author": "艾逗笔",
  "fileheader.LastModifiedBy": "艾逗笔",
  "fileheader.tpl": "/*\r\n * @Author: {author} <http://idoubi.cc>*/\r\n"
}
```
