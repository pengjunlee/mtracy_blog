> 原文链接：<https://blog.csdn.net/maixiaochai/article/details/88034709>

	#!/bin/bash
	# 这里是要被判断执行状态的命令（成功或者失败）
	some command 
	 
	# 这里是判断上条命令是否执行成功的语句块
	if [ $? -eq 0 ]; then
	    echo "succeed"
	else
	    echo "failed"
	fi
	 
	# linux 命令中，如果命令执行成功，则 $?值为 0，否则不为 0.
	# -eq 等于
	# -ne 不等于
	# -gt 大于
	# -lt 小于
	# -ge 大于等于
	# -le 小于等于