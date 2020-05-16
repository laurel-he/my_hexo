---
title: vim学习笔记
date: 2019-02-28 16:34:10
tags: 文档
categories: 快捷键
---
## 光标移动

h 或 <- 表示向左
j 下
k 上
l 右

ctrl + f     屏幕向下移动一页
ctrl + b     向上移动一页
ctrl + d     向下移动半页
ctrl + u     向上移动半页
'+'          光标移动到非空格符的下一行
'-'          光标移动到非空格符的
n[space]   n表示数字，光标向右移动n个字符
0或[Home]  移动到这一行最开始的字符处
$或[End]   移动到这一行的最后面字符处
H          移动到这个屏幕最上方一行的第一个字符
M          移动到这个屏幕中央一行的第一个字符
L          移动到这个屏幕最下方一行的第一个字符
G          移动到这个文件的最后一行
nG(:n)     移动到这个文件的第n行
gg(1G)     移动到第一行
n[Enter]   n为数字，表示向下移动n行

## 查找与替换

/word 光标之下寻找名为word的字符串
?word 光标之上寻找名为word的字符串
n     重复前一个查找的操作
N     反向进行前一个查找操作
:n1,n2s/word1/word2/g    n1和n2是数字，在n1和n2行之间查找所有word,替换成word2
:1,$s/word1/word2/g      
:1,$s/word1/word2/gc     c代表confirm,替换前显示确认信息
## 删除复制与粘贴
x       向后删除
X       向前删除
nx      向后删除n个字符
dd      删除（剪切）光标所在一整行
ndd     删除向下n行
d1G     删除光标所在到第一行的数据
dG      删除光标所在到最后一行的数据
d$      删除光标所在处，到该行最后一个字符
d0      
yy      复制该行
nyy
y1G
yG
y0
y$
p       将已复制的数据在光标下一行粘贴
P       上一行
J       光标所在行与下一行数据结合成同一行
c       重复删除多个数据(10cj代表向下删除10行)删除后切换到编辑模式
u       回复
ctrl+r  重做
.       重复前一次操作

## 进入插入或替换的编辑模式

i I     i从目前光标所在处插入，I目前所在行的第一个非空格字符处开始插入
a A     a 从目前光标所在的下一个字符处开始插入,A从光标所在行的最后一个字符处开始插入
o O     o 在目前光标所在行的下一行处插入新行  O上一行
r R     r 替换一次 R 一直替换直到按Esc

## 一般命令模式切换到命令模式

ZZ      若文件没有被修改则不保存退出，修改了则保存后退出
:w [filename] 将编辑的数据保存成另一个新文件
:r [filename] 读入另一个文件的数据，加在光标所在行后面
:n1, n2 w [filename] 将n1到n2的内容保存到文件
:! command 暂时退出vi到命令行模式下执行command的显示结果（:! ls /home）
:set nu
:set nonu
:e! 还原文件内容事实上，可以使用[ :! ls /home ]不过，如果你学过后面的章节之后，你会发现，执行[ ctrl + z ]亦可暂时退出 vi 让你在命令行

##多行添加数据

**方法一：块选择模式**
（1）v 进入virtual模式
（2）上下键选择需要注释的行数
（3）ctrl+v 进入列模式
（4）大写I进入插入模式，输入要编辑的符号
（5）立即按下两次ESC
**方法二：替换命令**
：起始行号，结束行号s/^/注释符/g
取消注释：
:起始行号,结束行号s/^注释符//g

####编辑多行

ctrl+v进入visual block模式，按两次j或者2j，将光标移动到要插入的最后一行，按下i进入编辑模式。输入内容，Esc退出

##多文件编辑

:n 编辑下一个文件
:N 编辑上一个文件
:files 列出目前这个vim打开的所有文件
（nyy复制后，使用p粘贴）

##多窗口功能

**分区窗口并放入文件**
:sp {filename}
#### 按键功能介绍
:sp [filename] 打开一个新窗口，如果加filename表示打开另一个文件，否则打开自身
[ctrl] + w + j 或[ctrl] + w + ↓：先按住ctrl键不放，按下w后放开，之后再按↓或↑
ctrl + w + q 相当于切换到某个窗口，按下:q离开，也可以直接ctrl+w+q

##vim的挑字补全功能

ctrl + x -> ctrl + n 通过正在编辑的这个文件的内容文字作为关键组予以补齐
ctrl + x -> ctrl + f 以当前目录的文件名作为关键字予以补齐
ctrl + x -> ctrl + o 以扩展名作为语法补充，以vim内置的关键字予以补齐

#### vim 自动补全报错option ‘omnifunc’ is not set

编辑vimrc文件，加入以下内容：
```
autocmd FileType python set omnifunc=pythoncomplete#Complete
autocmd FileType javascript set omnifunc=javascriptcomplete#CompleteJS
autocmd FileType html set omnifunc=htmlcomplete#CompleteTags
autocmd FileType css set omnifunc=csscomplete#CompleteCSS
autocmd FileType xml set omnifunc=xmlcomplete#CompleteTags
autocmd FileType php set omnifunc=phpcomplete#CompletePHP
autocmd FileType c set omnifunc=ccomplete#Complete
```

##编码转换

iconv list:列出所有的编码
iconv -f 原本编码 -t 新编码 filename [-o newfilw]
##vim的环境设置参数

:set no或者:set number
:set hlsearch 设置高亮度搜寻
:set nohlsearch
:set autoindent 自动缩进
:set noautoindent 设置不自动缩进
:set backup 设置自动储存备份文件，一般设置为nobackup，如果为backup，每次更改一个文件时会自动产生一个`filename~`文件记录原始文件内容
:set ruler 显示右下角状态列
:set showmode 是否要显示--INSERT--之类的字眼
:set backspace=(012) 某些distribution不允许使用backspace删除字符，当设置值为2可以删除任意值，为0或1时，只能删除刚才输入的
:set all 显示目前所有的环境参数设置值
:set 显示与系统默认值不同的设置参数
:syntax on 程序相关语法显示不同颜色
:syntax off 
:set bg=dark 显示不同的颜色色调
:set bg=light 编辑'~/.vimrc'文件，有或没有冒号都可以

##其他相关操作
**virtualbox添加共享文件**
1 设置-添加共享文件，选择固定分配
2 添加挂载：sudo mount -t vboxsf rezb2b /mnt/rezb2b
3 自启动挂载命令：sudo vim /etc/rc.local 添加命令
