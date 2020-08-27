> 原文地址： <https://www.iteye.com/blog/dannyhz-2412222>

`git pull`失败 ,提示：<font color=red>fatal: refusing to merge unrelated histories</font>

其实这个问题是因为两个根本不相干的`git`库，一个是本地库，一个是远端库，然后本地要去推送到远端，远端觉得这个本地库跟自己不相干，所以告知无法合并。

有两种方法来进行处理。

# 方法一
从远端库拉下来代码 ，将本地要加入的代码放到远端库下载到本地的库， 然后提交上去，因为这样的话，你基于的库就是远端的库，这是一次update了。

# 方法二
使用这个强制的方法。

	git pull origin master --allow-unrelated-histories

后面加上`--allow-unrelated-histories`， 把两段不相干的分支进行强行合并，后面再push就可以了 

	git push gitlab master:init

gitlab是别名， 使用：

	git remote add gitlab ssh://xzh@192.168.1.91:50022/opt/gitrepo/withholdings/WithholdingTransaction

- master是本地的branch名字
- init是远端要推送的branch名字

本地必须要先`add` ，`commit`完了才能推上去。

关于这个问题，可以参考<http://stackoverflow.com/questions/37937984/git-refusing-to-merge-unrelated-histories>。

在进行`git pull`时，添加一个可选项。

	git pull origin master --allow-unrelated-histories