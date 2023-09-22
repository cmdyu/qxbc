---
layout: post
title: vscode技巧总结
categories: vscode
---
> "Tips and Tricks" lets you jump right in and learn how to be productive with Visual Studio Code.

## 官方技巧
[Visual Studio Code Tips and Tricks](https://code.visualstudio.com/docs/getstarted/tips-and-tricks)

## 我发现的技巧
- 删除一个代码块的时候，先用 Ctrl + Shift + [ 折叠代码块，再选中然后删除，比较快捷
- 快捷键 Ctrl + L 能实现向下选中一行
- 可以用View: Focus Other Side in Active Editor 命令在用View: Toggle Split Editor in Group命令或Volar: Split <script>, <template>, <style> Editors命令进行组内拆分后的窗口间跳转
- 在命令输入框中输入 @: ，能分类列出当前文件中的符号（symbol），如classes、functions、variables等
- 用Volar: Vue: Find File References命令能列出引用了当前文件的文件
- 在命令输入框中输入以 # 开头的内容，就可以在工作区范围里全局搜索标识符，光标停留在某个标识符上时，按快捷键 Ctrl + T，可以实现在工作区范围内搜索该标识符的定义，这个对应的命令名叫：Go to Symbol in Workspace...
