---
title: 定制一个powersehll的prompt
date: 2024-09-21 13:40:00 +0800
categories: miscelleneous
tags: tools
description: 如果终端黑匣子不够清楚怎么办？那就美化它！
---

## 个性化你的 PowerShell 提示符

对于经常使用命令行的开发者来说，个性化命令行提示符无疑是一种提高效率、增加可读性的方法。

![](https://yeijon-note.oss-cn-beijing.aliyuncs.com/img/202409211358672.png)

这是我的 PowerShell 配置文件：

```powershell
Import-Module posh-git

function prompt {
  $currentTime = Get-Date -Format "HH:mm:ss"
  $status = git status -s 2>$null
  $branch = git rev-parse --abbrev-ref HEAD 2>$null

  if ($status) {
    $statusSymbols = "!"
  } else {
    $statusSymbols = ""
  }

  $location = (Get-Location | Get-Item).Name 

  if ($branch) {
    if ($statusSymbols) {
      $gitInfo = " [`e[38;5;197m$branch`e[0m `e[38;5;240m$statusSymbols`e[0m]"
    } else {
      $gitInfo = " [`e[38;5;197m$branch`e[0m]"
    }
  } else {
    $gitInfo = ""
  }

  "`e[38;5;214m:) `e[1;36m$currentTime`e[0m$gitInfo`e[0m `e[1;4m$location`e[0m`e[38;5;3m>`e[0m "
}
```

## PowerShell语法

### Import-Module
`Import-Module` 是 PowerShell 中加载模块的命令，这里引入了 `posh-git` 这个模块，它提供了 Git 的终端集成。

### Date and Time
在 PowerShell 中，可以使用 `Get-Date -Format "HH:mm:ss"` 来获取格式化后的当前时间。

### Location Information
`$location = (Get-Location | Get-Item).Name` 是获取当前目录名称的表达式。

### Redirect的使用
在 PowerShell 中，通过 `2>$null` 可以将错误输出重定向至 `$null`，达到忽略错误信息的目的。

## Git在Prompt中的应用

### Git Branch and Status
我们用到 Git 的两个关键命令：

- `git status -s`：获取简短的状态输出。
- `git rev-parse --abbrev-ref HEAD`：获取当前 Git 仓库所在分支的名称。

## 提供更多的颜色
在提示符中显示 Git 分支和状态信息。使用 `e[38;5;197m` 和 `e[38;5;240m` 表示文本颜色，实现 Git 分支和状态在提示符中的颜色区分。

> 其他Shell提示符内容
> 对于 bash 或 fish 等类型的 shell，有不同的命令和选项可以直接或者通过第三方插件进行提示符的个性化
{: .prompt-tip }

参考：
[ANSI Color Codes](https://talyian.github.io/ansicolors/)
[How to Prompt for Input in PowerShell?](https://www.sharepointdiary.com/2021/10/prompt-for-input-in-powershell.html)
