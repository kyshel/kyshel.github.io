---
tags: [shell]
title: shell快捷键探索之旅
---

快捷键相当重要，首要目的是 装B，效率是后话。

# 手动猜测
以前只知道Ctrl+l可以清屏，Ctrl+c可以强制终止当前命令，可是26个字母怎么可能只用到这么几个键，又不想查手册，手动来一遍猜猜把 (为方便，Ctrl+L用^l代替)：
- ^q 没反应
- ^w 删除光标之前的1个word（删到空格
- ^e end, 光标到末尾
- ^r reverse-i-search 什么鬼，似乎是查找历史命令
- ^t 置换光标处和光标前两个字符，这有啥用
- ^y 粘贴被删掉的字符，不知这个y什么意思
- ^u 删掉光标前所有字符，应该还有个光标后
- ^i tab补全的alternative way
- ^o 运行，但不删除当前命令，这个功能好，加参数啥的方便
- ^p preview，显示上一个命令，那么下一个应该是^n, next
- ^a ^e相反
- ^s 冻住当前命令? 此时打东西没反应，但再按^q会瞬间显示
- ^d 相当于delete键， 那也有backspace把
- ^f forward，光标向后移动1个字符，这样就不怕没有小键盘了
- ^g 没反应，但蜂鸣器一直在响
- ^h backspace
- ^j jump to command？ 目测相当于回车
- ^k ^u相反
- ^l 清屏，相当于clear，但会保存当前命令
- ^z 把正在运行的命令休眠，运行fg可以恢复
- ^x 光标很神奇地移动，不知是干啥的
- ^c 强制终止当前运行程序
- ^v 界面上直接出^V 
- ^b backward,^v相反
- ^n next character
- ^m 和^j一样？

# 查阅手册
- ^s Stop all output to the screen
- ^xx 是x两次，跳到开始处，再按跳回来
- ^y yank
- ^o ^r后用^o运行

# end
还是手动来一遍印象深刻

