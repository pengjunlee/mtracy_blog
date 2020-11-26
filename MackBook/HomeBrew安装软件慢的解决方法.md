用原有的镜像下载非常慢 => 🚶
替换源，更新速度变成 => 🚀

**第一步**：更换仓库源homebrew默认的源是在github上面，每次更新速度都会非常慢。所以我们更换成国内的镜像源。就会快很多了。

	cd "$(brew --repo)"
	git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
	 
	cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
	git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
	 
	cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
	git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-cask.git
	 
	brew update

**第二步**：更换`Homebrew-bottles`镜像（影响软件下载速度）
	
	# 临时替换
	export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles
 
	# 永久替换
	echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
	source ~/.bash_profile

参考链接:[清华homebrew源](清华homebrew源 "https://links.jianshu.com/go?to=https%3A%2F%2Fmirrors.tuna.tsinghua.edu.cn%2Fhelp%2Fhomebrew%2F")