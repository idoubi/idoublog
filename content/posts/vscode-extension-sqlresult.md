---
title: "开发一款转换sql查询结果的vscode插件"
date: 2021-03-19T16:23:08+08:00
draft: false
---

## 背景

做开发的同事应该很多人有过类似的经历：运营同事找过来，让我们帮忙导一份用户下单的数据。

我们一般的操作流程是：登录跳板机，连上数据库，输入查询 sql，复制 sql 查询结果，发给运营同事。

sql 查询结果一般长这样：
![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_1.png)

我们把查询结果保存到一个 txt 文件，发给运营同事，以为万事大吉了。

结果运营同事回这么一句：
![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_2.png)

运营同事用 windows 系统的记事本打开这个 txt 文件，看到的格式是乱的，他们希望我们给到的是一个 csv 或者 excel 表格文件。

作为一个开发，我们可能会想，这种小事情，写个插件来搞定吧。

所以进入到了今天的主题：

开发一个 vscode 插件，将 sql 查询结果转换成表格形式，并保存为 csv 文件。

## vscode 插件开发

vscode 插件开发的详细步骤我就不写了，贴一篇我看过的教程，有兴趣的同学可以移步 [这里](https://liiked.github.io/VS-Code-Extension-Doc-ZH/#/get-started/your-first-extension) 去看。

- 安装命令行

`npm i -g yo generator-code`

- 创建插件

在终端执行 `yo code`，根据提示一步一步输入插件信息之后，我创建了一个目录名为 `sqlresult` 的插件。

vscode 打开插件目录，可以看到自动生成的插件文件结构长这样：
![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_3.png)

其中 `package.json` 是插件信息文件，`src/extension.ts` 是插件的主逻辑。

- 定义插件子命令

修改 `package.json` ，定义一个保存文件内容到 csv 的命令：
![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_4.png)

在 `src/extension.ts` 文件里面注册命令，编写命令的运行逻辑：
![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_5.png)

- 插件调试

在编辑器按 F5，打开一个新的编辑器窗口调试插件功能。

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_6.png)

按 `command+shift+p` 弹出命令输入框，运行我们的自定义命令。

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_7.png)

命令执行后，通过 `vscode.window.showInformationMessage` 输出的调试信息在编辑器右下角弹出显示。

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_8.png)

通过 `console.log` 输出的调试信息在编辑器下方的调试控制台输出。

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_9.png)

调试搞定了，接下来就专注开发插件主逻辑了。

## 编写插件主逻辑

- 获取编辑器内容

我们需要获取到用户在编辑器里选择的文本内容，才能完成后续的转换

```ts
let editor = vscode.window.activeTextEditor;
let selection = editor?.selection;
let text = editor?.document.getText(selection);
```

- 内容替换

插件的核心功能是把编辑器选择的 sql 查询结果转换成能保存到 csv 文件的内容。
我们分析后发现，sql 查询结果通过大量的“|”来做内容的分割，而一个 csv 文件，用文本框打开后是用“,”分割单元格的。

所以我们要做的是把“|”替换成“，”，并且要去掉表头和表尾的分割线。

`sql result`

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_10.png)

`csv content`

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_11.png)

要做内容替换，比较简单的方式就是用正则表达式。

打开 [https://regex101.com/](https://regex101.com/)，粘贴我们要替换的内容，通过调试找到一个正确的正则表达式：`/\|((.*)\|){1,}/g` 可以用来做多行匹配。

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_12.png)

匹配后获得的每一行数据都是这种格式

`| 6f9fa41be49979251202a5cf23d500da | 1 | 2021-03-19 12:01:52 |`

我们只需要把“|”替换成“,”，再去除首尾的“,”就行了。

最后写出来的转换逻辑是这样：

```ts
let editor = vscode.window.activeTextEditor;
let selection = editor?.selection;
let text = editor?.document.getText(selection);

let arr = text?.match(/\|((.*)\|){1,}/g);
let newArr: string[] = [];

arr?.forEach(function (item, i) {
  let str = item.replace(/\s*\|\s*/g, ",");
  str = str.replace(/^\,/, "").replace(/\,$/, "");
  newArr.push(str);
});

let content = newArr.join("\n");
```

- 保存文件

完成内容转换后，下一步我们就可以写保存文件的逻辑了。vscode 已经封装了相关的 api，我们只需要通过几行代码即可完成调用。

```ts
// 打开文件保存对话框
let uri = await vscode.window.showSaveDialog({
  filters: {
    csv: ["csv"],
  },
});

if (uri == undefined) {
  vscode.window.showErrorMessage("invalid save path");
  return;
}

// 保存文件
let writeData = Buffer.from(content, "utf8");
vscode.workspace.fs.writeFile(uri, writeData);
```

- 完整逻辑

最后再加上必要的错误提示，和保存后打开文件的操作，这个插件的核心逻辑就写完了。

`src/extension.ts`

```ts
// The module 'vscode' contains the VS Code extensibility API
// Import the module and reference it with the alias vscode in your code below
import * as vscode from "vscode";

// this method is called when your extension is activated
// your extension is activated the very first time the command is executed
export function activate(context: vscode.ExtensionContext) {
  // Use the console to output diagnostic information (console.log) and errors (console.error)
  // This line of code will only be executed once when your extension is activated
  console.log('Congratulations, your extension "sqlresult" is now active!');

  // The command has been defined in the package.json file
  // Now provide the implementation of the command with registerCommand
  // The commandId parameter must match the command field in package.json
  let disposable = vscode.commands.registerCommand(
    "sqlresult.tocsv",
    async () => {
      let editor = vscode.window.activeTextEditor;
      let selection = editor?.selection;
      let text = editor?.document.getText(selection);

      let arr = text?.match(/\|((.*)\|){1,}/g);
      let newArr: string[] = [];

      arr?.forEach(function (item, i) {
        let str = item.replace(/\s*\|\s*/g, ",");
        str = str.replace(/^\,/, "").replace(/\,$/, "");
        newArr.push(str);
      });

      let content = newArr.join("\n");

      if (content == "") {
        vscode.window.showErrorMessage("invalid content");
        return;
      }

      let uri = await vscode.window.showSaveDialog({
        filters: {
          csv: ["csv"],
        },
      });

      if (uri == undefined) {
        vscode.window.showErrorMessage("invalid save path");
        return;
      }

      let writeData = Buffer.from(content, "utf8");
      vscode.workspace.fs.writeFile(uri, writeData);

      // Display a message box to the user
      let openFile = "Open File";
      vscode.window
        .showInformationMessage("file saved", openFile)
        .then((value) => {
          if (value === openFile) {
            vscode.window.showTextDocument(uri!, {});
          }
        });
    }
  );

  context.subscriptions.push(disposable);
}

// this method is called when your extension is deactivated
export function deactivate() {}
```

## 插件使用

在 vscode 扩展中心搜索`sqlresult`并安装插件

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_13_1.png)

安装完插件，复制 sql 查询结果到编辑器，全选内容，执行插件保存 csv 文件的命令：

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_14_1.png)

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_15.png)

打开保存后的 csv 文件，内容看起来非常工整。

![enter image description here](http://blogcdn.idoustudio.com/pic/sqlresult_16.png)

最后把 csv 文件发给运营同事，就可以收工了。

## 总结

本文从实际需求出发，写了一个能帮助开发者日常导数据的小插件。vscode 是一个“真香”神器，丰富的插件给了开发者很多想象的空间，强烈安利一波。

插件源码奉上，欢迎赐教：[https://github.com/idoubi/sqlresult](https://github.com/idoubi/sqlresult)
