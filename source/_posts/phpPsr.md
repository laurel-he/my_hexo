---
title: 安装php sniffer
date: 2019-04-07 18:27:44
tags:
categories: 
- php
---

###  安装pear（参考文档：https://www.cnblogs.com/huangbx/p/php_codesniffer.html）
-------------
1. 从以下网址：http://pear.php.net/go-pear.phar下载代码，或直接另存为文件go-pear.phar
2. 运行php go-pear.phar（注意是否加了环境变量，没有可在php.exe目录下运行）
3. 如果报错“ERROR, enter the php.exe path”，输入选项13，选择php.exe所在目录

### 安装php sniffer
------------
1. 运行命令“pear install PHP_CodeSniffer”
2. 配置phpstorm（其他IDE请自行Google），Settings->Languages & Frameworks->php->Code Sniffer，选择刚才安装的phpcs.bat文件，validate 看看会不会报错
3. 如果校验报错，可能是找不到autoload路径，打开phpcs文件，更改include路径，例如我可以直接改成include_once '/pear/PHP/CodeSniffer/autoload.php'
4. 选择Settings->Inspections->PHP Code Sniffer validation，点击刷新按钮，选择"PSR2"
5. 此时如果有不规范的代码可以看到有phpcs的报错了

### 配置本地git
----------
1. 打开.git/hooks,新建文件pre-commit
2. 示例代码如下
```
#!/bin/bash
#
# check PHP code syntax error and standard with phpcs
# author : star[github.com/star1989]
# date : 2017-02-24
PROJECT=$(git rev-parse --show-toplevel)
cd $PROJECT
SFILES=$(git diff --cached --name-only --diff-filter=ACMR HEAD | grep \\.php)
TMP_DIR=$PROJECT."/tmp"

# Determine if a file list is passed
if [ "$#" -ne 0 ]
then
    exit 0
fi
echo "Checking PHP Lint..."
for FILE in $SFILES
do
#    echo "php -l -d display_errors=0 ${FILE}"
#   echo "git show :$FILE > $TMP_DIR/$FILE"
    php -l -d display_errors=0 $FILE
    if [ $? != 0  ]
    then
        echo "Fix the error before commit."
        exit 1
    fi
    FILES="$FILES $PROJECT/$FILE"
done

if [ "$FILES" != "" ]
then
    echo "Running Code Sniffer..."

    TMP_DIR=/tmp/$(uuidgen)
    mkdir -p $TMP_DIR
    for FILE in $SFILES
    do
        mkdir -p $TMP_DIR/$(dirname $FILE)
        git show :$FILE > $TMP_DIR/$FILE
    done
    phpcs --standard=PSR2 --encoding=utf-8 -n $TMP_DIR
    PHPCS_ERROR=$?
    rm -rf $TMP_DIR
    if [ $PHPCS_ERROR != 0 ]
    then
        echo "Fix the error before commit."
        exit 1
    fi
fi

exit $?
```
3. 注意这行phpcs --standard=PSR2 --encoding=utf-8 -n $TMP_DIR，在运行git的终端下运行phpcs --help,如果可以运行这行才会生效
4. 此时提交不规范的代码就会报错
