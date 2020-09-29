## mkdir

[p] [dirname] 创建目录

- mkdir -p foo 确保 foo 目录存在，否则创建

## touch

[option] [file] 创建文件，并且可以修改文件的时间属性

- touch file1 file2 …… 创建多个文件

## stat

[file] 查看文件属性状态信息

- stat file

## chmod

[-cfvR] [file] 设定文件权限

- chmod 777 foo 设定 foo 文件的权限为：所有者的权限、用户组的权限、其它用户的权限均为 7（读+写+执行）

## find

<dir> -name "xxx"
- find ./ -name "*.[ch]"
- find ./ -regex ".*\.java\|.*\.xml" 正则匹配
- find ./ -name "*.java" -o -name "*.xml"

## ln

ln -s <fromPath> <toPath> 创建软链接

## ls

- -a 不隐藏以 . 开始的项
- -A 列出除 . 与 .. 以外的项
- -d \*/ 列出当前所有目录
- -S 根据文件大小排序
- -h 与-l 一起，以易于阅读的格式输出文件大小，且基数 1000，非 1024
- -lh #不以字节方式显示文件大小
- -l --block-size=M #使用 MB 作为单位，文件大小
- -n #打印 UID 和 GID
- -p #目录增加 / (斜线)
- -r #按时间倒排序
- -R #递归列出子目录
- -X #扩展名排序
- -t #按修改时间倒排序

## ps

Process Status，获取进程状态。

- ps -ef #显示所有进程信息的快照
- ps -elf #

## kill

发送指定的信号到相应进程。

- kill -l #列出所有信号名称
- kill -9 \$(ps -ef | grep peidalinux) #杀死过滤后的所有进程
- kill -u <username> #杀死指定用户的所有进程

#### 常用信号

| 名称 | 信号 | 说明                              |
| ---- | ---- | --------------------------------- |
| HUP  | 1    | 终端断线                          |
| INT  | 2    | 中断（同 Ctrl + C）               |
| QUIT | 3    | 退出（同 Ctrl + \）               |
| KILL | 9    | 强制终止，无条件终止进程          |
| TERM | 15   | 终止                              |
| CONT | 18   | 继续（与 STOP 相反， fg/bg 命令） |
| STOP | 19   | 暂停（同 Ctrl + Z）               |

> 如果没有指定信号，kill 命令就会发出终止信号(15)，这个信号可以被进程捕获，使得进程在退出之前可以清理并释放资源

## grep

Global Regular Expression Print，文本搜索工具。
匹配模式选择

- -e, --regexp=PATTERN 后面根正则模式，默认无
- -i, --ignore-case 不区分大小写
- -w, --word-regexp 匹配整个单词
- -x, --line-regexp 匹配整行

杂项

- -s, --no-messages 不显示错误信息
- -v, --invert-match 显示不匹配的行
- -n 显示行号
- -c 显示匹配的个数
- -m <number> 显示几条匹配记录
- -h 显示行号不显示文件名
- -l 仅显示文件名
- -o 匹配一行中多个（仅显示匹配内容）
- -R 在有层级目录中，递归的显示所有匹配内容
- --color 高亮匹配字符串

示例

- grep '^\(hello\|world\)' 匹配两个词，-e 默认是省去的，注意反斜杠
- grep '^zhang[a-z]\*\$' 匹配到 zhangying
- grep '[^g]oo' 匹配无 g 的 oo
- grep '^\$' 匹配空行
- grep 'e\{2\}' 匹配出现 2 次 e，如需匹配更多 `\{2,\}`

> 使用 egrep 指令时，不用加 \ 转义；使用 () 目的是形成单元组，方便是用 +?\* 等操作；

## cut

对每行进行文本字符的截取操作。
-b <n-n> 输出前 n 个字节
-c <n,n|n-n> 输出前 n 个，n 到 n，第 n 个和第 n 个字符,
-d <"delim"> 使用 delim 取代 Tab 做分隔符解析
-f <n,n> 输出第 n 列字段值

## head

-c 显示前 n 字节内容
-n 显示前 n 行内容

## lsof

- lsof -i:<port> #根据端口查进程
- lsof -i port:<port> #根据端口查进程

## netstat

- netstat -anp #查网络相关信息

## mv

- mv dirA dirB #目录更名

## wc

统计指定文件中的字节数、单词数、行数, 并将统计结果显示输出

## xargs

又称管道命令，构造参数。

## 查看系统内核信息

- cat /proc/version
