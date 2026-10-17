---
layout: post
title: 解决 PowerShell 提示“此系统上禁止运行脚本”问题
date: 2026-10-17
categories: 问题解决
tags: 问题解决 DSH
---

在安装 Deepseek Harness 时，我在 PowerShell 中执行了如下命令

    npm install -g @deepseek-ai/dsh

但却遇到系统提示“此系统上禁止运行脚本”
![1](/assets/img/2026/10-17/1.png)

查了下发现这是因为 **PowerShell 的执行策略（Execution Policy）禁止了脚本运行**。PowerShell 默认的执行策略是  `Restricted` ，它会阻止所有 `.ps1` 脚本的执行。

执行如下命令即可查看 PowerShell 当前的执行策略

    Get-ExecutionPolicy

![2](/assets/img/2026/10-17/2.png)

我们可将执行策略改为 `RemoteSigned` ，它允许本地脚本运行，但对从网络下载的脚本要求数字签名，在安全性和可用性之间取得平衡。执行如下命令以修改执行策略。其中`-Scope CurrentUser`参数表明只对当前用户生效，无需管理员权限。

    Set-ExecutionPolicy RemoteSigned -Scope CurrentUser

![3](/assets/img/2026/10-17/3.png)

在完成上述操作之后便可正常执行在开篇提到的安装命令了。