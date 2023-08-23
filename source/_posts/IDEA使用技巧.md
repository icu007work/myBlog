---
title: IDEA使用技巧
author: Rookie_l
categories: 技术
category_bar: true
tags:
  - IDEA
  - 随笔
abbrlink: 419d96b2
cover: 'https://pic.ziyuan.wang/2023/08/23/31c4c2e2cd96d.jpg'
ai:
  - 这篇文章介绍了关于使用IntelliJ IDEA的一些实用技巧，涵盖了以下几个主要方面：入门导览、基本操作、编辑器基础知识、代码补全、重构、代码辅助、导航以及Git集成；这篇文章提供了丰富的示例和截图，帮助读者更好地理解每个技巧的操作步骤，有助于提高在开发过程中的效率和便捷性。
date: 2023-07-29 15:30:11
keywords:
description:
---
# IDEA使用技巧

## 一、入门导览

- 按住 `Alt + 1`即可打开项目视图，双击源码即可打开文件。

![1690527585832.png](https://pic.ziyuan.wang/2023/07/28/4fa81c51a8b5b.png)

- 按下`Shift + F10` 即可运行项目，旁边的小虫子即为调试模式-

![1690527632047.png](https://pic.ziyuan.wang/2023/07/28/5b7369330558a.png)

- 在`for循环`处，按下 `Alt + Enter`即可将`for循环`优化为`增强for循环`，在打印语句中按下 `Alt + Enter`即可 `+` 号替换成 `String.format`

![1690528026893.png](https://pic.ziyuan.wang/2023/07/28/5d8a51a96c98e.png)![1690528134066.png](https://pic.ziyuan.wang/2023/07/28/da96868e287cd.png)

- 双击 `Shift`即可触发全局搜索。

![1690528240450.png](https://pic.ziyuan.wang/2023/07/28/0096bcd4dc133.png)

---

## 二、基本

### 2.1 上下文操作

- 同样的在需要操作的代码处，按下 `Alt + Enter`即可显示上下文操作。

![1690528356924.png](https://pic.ziyuan.wang/2023/07/28/9e4ae19c8e262.png)

![1690528461547.png](https://pic.ziyuan.wang/2023/07/28/536470d6d8bdb.png)

---

### 2.2 搜索操作

- 可以按下 `Ctrl + Shift + A` 或连按两次 `Shift`触发搜索。

![1690528644744.png](https://pic.ziyuan.wang/2023/07/28/a34515b5d40dc.png)

- `Ctrl + N`搜索类， `Ctrl + Q` 预览所选类的文档

![1690528829830.png](https://pic.ziyuan.wang/2023/07/28/a2928125e6403.png)

- 总结
  - `Ctrl + N`：搜索类
  - `Ctrl + Shift + N`：搜索文件
  - `Ctrl + Shift + Alt + N`：搜索符号
  - `Ctrl + Shift + A`：搜索操作
  - 双击 `Shift`：全局搜索

---

### 2.3 基本补全

- `Ctrl + 空格`：触发基本补全，`Ctrl + Shift + Enter`：补全当前语句。

![1690529434725.png](https://pic.ziyuan.wang/2023/07/28/07a5fa14fea3b.png)

- `Ctrl + 两次空格`：查看有关静态变量或方法的建议

![1690529682757.png](https://pic.ziyuan.wang/2023/07/28/4f7170be5a781.png)

----

## 三、编辑器基础知识

### 3.1 扩展和收缩代码选取

- 按 `Ctrl + W`可选择文本光标处的单词。
- 再次按`Ctrl + W`可选择整个字符串。
- 第三次按`Ctrl + W`以在选择中添加引号。
- 再按`Ctrl + W`两次可选择整个调用。
- 如果要选择它的实参，而不是选择整个调用。可按 `Ctrl + Shift + W` 将选区收缩到实参。
- 在if语句的开头。可按两次 `Ctrl + W` 将其选中。只需按几下，即可很好地将关键字作为选择对应语句的一个起点。

- 总结：`Ctrl + W`：扩选 ； `Ctrl + Shift + W`：缩选。

---

### 3.2 注释行和代码块

- `Ctrl + /`： 单行注释/取消单行注释
- `Ctrl + Shift + /`：多行注释/取消多行注释

---

### 3.3 复制和删除行

- `Ctrl + D`： 复制光标所在行
- `Ctrl + Y`： 删除光标所在行
- `Ctrl + ↑`： 向上选择

---

### 3.4 移动代码段

- `Alt + Shift + ↑`： 当前行向上移动
- `Alt + Shift + ↓`： 当前行向下移动
- `Ctrl + Shift + ↑`： 当前整个方法向上移动
- `Ctrl + Shift + ↓`： 当前整个方法向下移动

---

### 3.5 收起代码块

- `Ctrl + -`：收起当前代码块
- `Ctrl + =`：展开当前代码块
- `Ctrl + Shift + -`：收起文件中的所有区域
- `Ctrl + Shift + =`：展开文件中的所有区域

---

### 3.6 包围和解包

- `Ctrl + Alt + T`：使用一些模板代码包围代码段

![1690532033070.png](https://pic.ziyuan.wang/2023/07/28/ed25d369395be.png)

- `Ctrl + Alt + Delete`：解除模板代码包围

![1690532082762.png](https://pic.ziyuan.wang/2023/07/28/b6e236a6d4e9b.png)

---

### 3.7 多选

```html
<!doctype html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>Multiple selections</title>
    </head>
    <body>
        <table>
            <tr>
                <th>Firstname</th>
                <th>Lastname</th>
                <th>Points</th>
            </tr>
            <tr>
                <th>Eve</th>
                <th>Jackson</th>
                <th>94</th>
            </tr>
        </table>
    </body>
</html>

```

- 按 `Alt + J`可选择文本光标处的符号
- 再次按`Alt + J`可选择此符号的下一个匹配项
- 按 `AIt + Shift + J`可取消选择上一个匹配项
- 按 `Ctrl + Alt + Shift + L`可选择文件中的所有匹配项。
- 键入 `td`，将 `th`的所有匹配项替换为`td`

![1690532511051.png](https://pic.ziyuan.wang/2023/07/28/02a63a138c362.png)

---

## 四、代码补全

### 4.1 基本补全

- `Ctrl + 空格`：触发基本补全，`Ctrl + Shift + Enter`：补全当前语句。

![1690529434725.png](https://pic.ziyuan.wang/2023/07/28/07a5fa14fea3b.png)

- `Ctrl + 两次空格`：查看有关静态变量或方法的建议

![1690529682757.png](https://pic.ziyuan.wang/2023/07/28/4f7170be5a781.png)

---

### 4.2 类型匹配补全

- `Ctrl + Shift + 空格`：查看匹配当前类型的建议列表
- `Ctrl + Shift + 空格`还可以为 `return`提供代码建议

---

### 4.3 后缀补全

- 后缀补全有助于在编写代码时减少向后跳转文本光标。使用它可以根据添加的后缀、表达式的类型及其上下文，将已键入的表达式转换另一种表达式。 在圆括号后面键入`.`，以查看后缀补全建议列表。

![1690534971158.png](https://pic.ziyuan.wang/2023/07/28/ed61e269d482d.png)

---

### 4.4 语句补全

- `Ctrl + Shift + Enter`：可以补全类似 `for` 、 `if` 、 `switch`等语句

---

### 4.5 Tab补全

- 所有补全都可以搭配`Tab`使用

---

## 五、重构

### 5.1 重命名

- `Shift + F6`：重命名**所有与选中内容一致**的代码。

![image-20230728173111078](C:\Users\Charlie\AppData\Roaming\Typora\typora-user-images\image-20230728173111078.png)

- IDEA还会检测相应的 getter/setter，并提出相应的重命名建议。

---

### 5.2 提取变量

- `Ctrl + Alt + V`：可以提取局部变量，并且会自动匹配代码中所有与选中变量一致的变量。

![1690536888565.png](https://pic.ziyuan.wang/2023/07/28/a845344f7a8ef.png)

---

### 5.3 提取方法

- `Ctrl + Alt + M`：将所选代码块提取为方法

---

### 5.4 重构菜单

- `Ctrl + Shift + Alt + T`：列出当前上下文中可用的所有重构

![1690537208428.png](https://pic.ziyuan.wang/2023/07/28/a0c80aecc4800.png)

---

## 六、代码辅助

### 6.1 还原移除的代码

- 假设开发过程中需要还原先前删除的代码。因为此后发生了多项更改，撤消不起作用，而此时又不希望丢失这些更改。可点击几次 **本地历史记录** 来还原已删除的代码。在编辑器中的任意位置点击鼠标右键即可打开上下文菜单。

![1690537789293.png](https://pic.ziyuan.wang/2023/07/28/496d7ecb83c99.png)

---

### 6.2 格式化代码

- `Ctrl + Alt + L`：更正代码格式，若有选中的代码则更正当前选中的代码；若未选中代码，则重新格式化整个文件。
- `Ctrl + Alt + Shift + L`：显示重新格式化设置

![1690538025154.png](https://pic.ziyuan.wang/2023/07/28/c2d55dddcfde6.png)

---

### 6.3 形参信息

- `Ctrl + P`：查看方法签名

![1690538025154.png](https://pic.ziyuan.wang/2023/07/28/d8f002a921c1f.png)

---

### 6.4 快速弹出窗口

- 按 `Ctrl + Q`可查看文本光标处符号的文档。

![1690538637711.png](https://pic.ziyuan.wang/2023/07/28/8fde9ce454399.png)

- 按`Ctrl + Shift + l` 可查看文本光标处符号的定义。

![1690538613744.png](https://pic.ziyuan.wang/2023/07/28/6551090e183d2.png)

---

### 6.5 编辑器编码辅助

- `F2`： 转到文件中下一个高亮显示的错误。
- `Ctrl F1`：展开警告说明。
- 另一种有用的工具是高亮显示用法。按 `Ctrl + Shift + F7` 可高亮显示文件中文本光标处符号的所有用法。

---

## 七、导航

### 7.1 随处搜索

- `Ctrl + N`：搜索类
- `Ctrl + Shift + N`：搜索文件
- `Ctrl + Shift + Alt + N`：搜索符号
- `Ctrl + Shift + A`：搜索操作
- 双击 `Shift`：全局搜索

---

### 7.2 查找与替换

- `Ctrl + Shift + F`：全局搜索
- `Ctrl + Shift + R`：打开 在文件中替换窗口

---

### 7.3 文件重构

- `Ctrl + F12`：打开文件结构，打开后可键入单词以查找相关方法。
- `Alt + 7`：将文件结构显示为工具窗口

---

### 7.4 声明和用法

- `Ctrl + B`：在方法调用处，按下可跳转到方法声明
- `Ctrl + B`：在方法声明处，按下可查看其所有用法。

![1690599111074.png](https://pic.ziyuan.wang/2023/07/29/3929cedb1b7c9.png)

- `Alt + F7`：查看更详细的用法视图，浏览完后按`Shift + Esc`：隐藏视图，按 `Alt + 3`：再次打开查找视图

![1690599187782.png](https://pic.ziyuan.wang/2023/07/29/5d41571606bd4.png)

---

### 7.5 继承层次结构

- `Ctrl + Alt + B`：查找某个接口的实现
- `Ctrl + U`：从派生导航到 `super`方法
- `Ctrl + H`：查看某个类的层次结构
- `Ctrl + Shift + H`：查看某个方法的层次结构
- 注意：也可以对类执行 `Ctrl + Alt + B`和`Ctrl + U`操作

---

### 7.6 最近的文件和位置

- `Ctrl + E`：显示最近打开的文件

![1690600123613.png](https://pic.ziyuan.wang/2023/07/29/5ce656e84c878.png)

- `Ctrl + Shift + E`：显示最近打开文件内的代码视图

![1690600156977.png](https://pic.ziyuan.wang/2023/07/29/4455b3f3dbcd9.png)

---

### 7.7 下一个/上一个匹配项

- `Ctrl + F`：在当前文件中执行全文搜索。
- `F3 / Enter`：查找下一个匹配项
- `Shift + F3`：跳转到上一个匹配项
- 注意，在查找面板关闭的情况下仍可使用上述快捷键跳转匹配项

---

## 八、运行并调试

### 8.1 运行配置

- 点击代码左上角的![1690600625871.png](https://pic.ziyuan.wang/2023/07/29/eb27041a07a1a.png)是创建一个临时运行配置

![1690600566371.png](https://pic.ziyuan.wang/2023/07/29/759a51529fa14.png)

- 我们可以选择保存配置，这样配置就保存为 `Sample with parameters`了

![image-20230729111841097](C:\Users\Charlie\AppData\Roaming\Typora\typora-user-images\image-20230729111841097.png)

![1690600750761.png](https://pic.ziyuan.wang/2023/07/29/436ab8e60a59c.png)

- 要使同事可以访问运行配置，可将其存储为单独的文件并通过版本控制系统共享此文件。

![1690600509040.png](https://pic.ziyuan.wang/2023/07/29/e48b5ab33a1cd.png)

### 8.2 调试工作流

- `Ctrl + F8`：在当前行打断点
- `Shift + F9`：使用当前所选运行配置开始调试
- 调试时可以在搜索框调用函数表达式来检查某个函数是否抛出异常。

![1690604094195.png](https://pic.ziyuan.wang/2023/07/29/9687714379594.png)

- 当表达式导致异常时 可点击![1690611152951.png](https://pic.ziyuan.wang/2023/07/29/f00e7417cfe29.png)或者按下 `Ctrl + Shift + A`将该表达式添加到监听。

![1690611103971.png](https://pic.ziyuan.wang/2023/07/29/fc6b03ae7bbbc.png)

- `F7`： 步入
- `F8`： 步过
- `F9`： 恢复程序
- `Shift + F8`: 步出
- `Ctrl + Alt + F8`： 为所选实参调用“对表达式快速求值”
- 我们可以在修复后重新运行我们的小程序，但是对于大程序，重新运行可能需要很长时间。如果修复只影响纯方法，我们可以重新构建项目并应用热交换，而不是重新运行。按 `Ctrl + F9` 构建项目。
- `Alt + F9`： 执行程序直到文本光标所在的行。

---

## 九、Git

### 9.1 快速入门

- 双击 `Shift`并搜索克隆 打开克隆视图，输入URL即可克隆任意public git仓库的代码

![1690614101281.png](https://pic.ziyuan.wang/2023/07/29/6341ad8d942c7.png)

- `Ctrl + Alt + N`：新建分支

![1690614259677.png](https://pic.ziyuan.wang/2023/07/29/caf261acd55be.png)

- 提交界面

![1690614333074.png](https://pic.ziyuan.wang/2023/07/29/d702ab66db582.png)

- `Ctrl + Shift + K`： 推送代码

![1690614382744.png](https://pic.ziyuan.wang/2023/07/29/7c3a22be6d469.png)

---

### 9.2 项目历史记录

- `Alt + 9`：打开Git工具窗口

![1690614558981.png](https://pic.ziyuan.wang/2023/07/29/8d86935cfafae.png)

---

### 9.3 提交

- `Ctrl + K`: 打开提交工具窗口

![1690614724472.png](https://pic.ziyuan.wang/2023/07/29/5717b7cf44ec8.png)

- `Alt + 9` 打开Git工具窗口即可看到提交

![1690614842835.png](https://pic.ziyuan.wang/2023/07/29/ae562005aa6ff.png)

---

