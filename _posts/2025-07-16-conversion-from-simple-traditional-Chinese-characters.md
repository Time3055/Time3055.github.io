---
layout: post
title: "ChatGPT客户端窗口的简繁转换"
date: 2025-07-14
---


问题： 

ChatGPT语音输入的时候AI自动转换是有时候识别简体有时候识别繁体的。

我需要在ChatGPT的输入框里把文字统一一下。



解决方式：

AutoHotKey【版本是1.1哦】

简体转繁体和繁体转简体是不同的快捷键，运行两个ahk就好了。

目前设置的例子是Ctrl + Shift + T 触发繁体转简体。

0 = 简体→繁体；1 = 繁体→简体。

代码：

```autohotkey

^+t::

    ; 保存剪贴板内容

    ClipSaved := ClipboardAll

    Clipboard := ""

    Send ^x  ; 剪切选中的文字

    ClipWait, 1



    if (Clipboard = "") {

        TrayTip,, 无法读取剪贴板内容。, 3

        return

    }



    try {

        wordApp := ComObjCreate("Word.Application")

        doc := wordApp.Documents.Add()

        wordApp.Visible := false

        doc.Content.Text := Clipboard

        doc.Content.TCSCConverter(1, true, false)  ; 1 = 繁体→简体

        converted := doc.Content.Text

        doc.Close(false)

        wordApp.Quit()

		

	converted := StrReplace(converted, "`r", "")

        converted := StrReplace(converted, "`n", "")



        Sleep 50

        SendInput %converted%  

        Sleep 50

        Clipboard := ClipSaved

    }

    catch e {

        TrayTip,, 转换失败：" . e, 5

    }

return

```
