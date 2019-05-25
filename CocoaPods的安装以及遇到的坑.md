# cocoapods的安装以及遇到的坑
2017-06-12

### 一. cocoapods的安装

##（1）、将Ruby 的软件源替换成国内的

在这我用的是Ruby China 社区专注维护的这个源（https://gems.ruby-china.org/），保证环境中只存在一个软件源！

＊首先，执行以下命令删除原来的ruby源：`gem sources --remove https://rubygems.org/`

执行命令后可在终端看见以下信息：`https://rubygems.org/ removed from sources`

＊然后下一步添加你找到的可用的镜像源：`gem sources -a https://gems.ruby-china.com/`

＊验证新源是否替换成功：`gem sources -l`

终端输出:
	
	*** CURRENT SOURCES ***
	https://gems.ruby-china.com/

到此ruby 源替已经换成国内的源。

##（2）、开始安装 cocoapods

其实就是执行 `sudo gem install cocoapods` 命令这么简单，但这一步是最容易出现坑的。

=========可能出现的状况（坑）=========

问题一：
`While executing gem ... (Errno::EPERM)  Operation not permitted - /usr/bin/fuzzy_match` 错误

解决方案 ：

执行`sudo gem install -n /usr/local/bin cocoapods`  语句。然后提示`gems installed`即可。

问题二：
`Error installing pods:active support requires Ruby version >= 2.2.2`

解决方案 ：

查看ruby版本

`$ruby -v`

终端会输出你的ruby 版本信息

查看目前的所有ruby版本：

`$rvm list known`

如果提示`command not found` 

请先安装rvm

`$curl -L get.rvm.io | bash -s stable`

如果已安装会列出所有的ruby版本：

	MRI Rubies
	[ruby-]1.8.6[-p420]
	[ruby-]1.8.7[-head] # security released on head
	[ruby-]1.9.1[-p431]
	[ruby-]1.9.2[-p330]
	[ruby-]1.9.3[-p551]
	[ruby-]2.0.0[-p648]
	[ruby-]2.1[.8]
	[ruby-]2.2[.4]
	[ruby-]2.3[.0]
	[ruby-]2.2-head
	ruby-head
	......
安装2.2.2:

`$rvm install 2.2.2`

终端运行结果：（如果直接成功请绕过homebrew的卸载安装）

	Searching for binary rubies, this might take some time.
	Found remote file https://rvm_io.global.ssl.fastly.net/binaries/osx/10.11/x86_64/ruby-2.2.2.tar.bz2
	Checking requirements for osx.
	About to install Homebrew, press `Enter` for default installation in `/usr/local`,
	type new path if you wish custom Homebrew installation (the path needs to be writable for user)
	:

回车:

	It appears Homebrew is already installed. If your intent is to reinstall you
	should do the following before running this installer again:
	ruby -e "$(curl -fsSL  
	"The current contents of /usr/local are .git
	Requirements installation failed with status: 1.

出现Requirements installation failed with status: 1.时才执行：

`$ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/uninstall)"`

卸载home-brew

	Warning: This script will remove:/Library/Caches/Homebrew//usr/local/.git/
	Are you sure you want to uninstall Homebrew? [y/N] y
	==> Removing Homebrew installation...
	==> Removing empty directories...
	==> Homebrew uninstalled!
	You may want to restore /usr/local's original permissions
	  sudo chmod 0755 /usr/local
	  sudo chgrp wheel /usr/local

再执行：

`$ rvm install 2.2.2`	

提示：
	
	Searching for binary rubies, this might take some time.
	Found remote file https://rvm_io.global.ssl.fastly.net/binaries/osx/10.11/x86_64/ruby-2.2.2.tar.bz2
	Checking requirements for osx.
	About to install Homebrew, press `Enter` for default installation in `/usr/local`,
	type new path if you wish custom Homebrew installation (the path needs to be writable for user):	

按回车：

	==> This script will install:
	/usr/local/bin/brew
	/usr/local/Library/...
	/usr/local/share/doc/homebrew
	/usr/local/share/man/man1/brew.1/usr/local/share/zsh/site-functions/_brew
	/usr/local/etc/bash_completion.d/brew
	
	Press RETURN to continue or any other key to abort
	==> /usr/bin/sudo /bin/mkdir /Library/Caches/Homebrew
	Password:

这里需要输入电脑密码：

	==> /usr/bin/sudo /bin/chmod g+rwx /Library/Caches/Homebrew
	==> /usr/bin/sudo /usr/sbin/chown haha /Library/Caches/Homebrew
	==> Downloading and installing Homebrew...
	remote: Counting objects: 501, done.
	remote: Compressing objects: 100% (445/445), done.
	remote: Total 501 (delta 29), reused 360 (delta 27), pack-reused 0
	Receiving objects: 100% (501/501), 787.83 KiB | 169.00 KiB/s, done.
	Resolving deltas: 100% (29/29), done.
	From https://github.com/Homebrew/brew
	 * [new branch]      master     -> origin/master
	HEAD is now at 32f7e73 download_strategy: ensure fixed commit hash length
	==> Tapping homebrew/core
	Cloning into '/usr/local/Library/Taps/homebrew/homebrew-core'...
	remote: Counting objects: 3714, done.
	remote: Compressing objects: 100% (3598/3598), done.
	remote: Total 3714 (delta 14), reused 2112 (delta 6), pack-reused 0
	Receiving objects: 100% (3714/3714), 2.88 MiB | 240.00 KiB/s, done.
	Resolving deltas: 100% (14/14), done.
	Checking connectivity... done.
	Checking out files: 100% (3717/3717), done.
	Tapped 3591 formulae (3,740 files, 9.0M)
	==> Installation successful!
	==> Next steps
	Run `brew help` to get started
	Further documentation: https://git.io/brew-docs
	==> Homebrew has enabled anonymous aggregate user behaviour analytics
	Read the analytics documentation (and how to opt-out) here:
	https://git.io/brew-analytics
	Installing requirements for osx.
	Updating system.....
	Installing required packages: autoconf, automake, libtool, pkg-config, libyaml, readline, libksba, openssl........
	Certificates in '/usr/local/etc/openssl/cert.pem' are already up to date.
	Requirements installation successful.
	ruby-2.2.2 - #configure
	ruby-2.2.2 - #download
	  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
	                             Dload  Upload   Total   Spent    Left  Speed
	100 6854k  100 6854k    0     0  61342      0  0:01:54  0:01:54 --:--:--  132k
	ruby-2.2.2 - #validate archive
	ruby-2.2.2 - #extract
	ruby-2.2.2 - #validate binary
	ruby-2.2.2 - #setup
	ruby-2.2.2 - #gemset created /Users/haha/.rvm/gems/ruby-2.2.2@global
	ruby-2.2.2 - #importing gemset /Users/haha/.rvm/gemsets/global.gems..............................
	ruby-2.2.2 - #generating global wrappers........
	ruby-2.2.2 - #gemset created /Users/haha/.rvm/gems/ruby-2.2.2
	ruby-2.2.2 - #importing gemsetfile /Users/haha/.rvm/gemsets/default.gems evaluated to empty gem list
	ruby-2.2.2 - #generating default wrappers........
	Updating certificates in '/etc/openssl/cert.pem'.
	mkdir: /etc/openssl: Permission denied
	mkdir -p "/etc/openssl" failed, retrying with sudo
	haha password required for 'mkdir -p /etc/openssl': 
	and sudo mkdir worked	

至此ruby2.2.2就安装好了，也可以直接安装更高的版本，只需将版本号修改一下

然后我们继续sudo gem install cocoapods

问题三、Setting up CocoaPods master repo 卡着不动

出现Setting up CocoaPods master repo，说明Cocoapods正在将它的信息下载到 ~/.cocoapods里；（这一步是很费时间的，等输出Setup completed 安装完成啦）

在此过程中可以右击终端选 --->新建窗口；在新建的终端窗口输入：

`cd ~/.cocoapods`

进入cocoa pods文件，然后在终端输入：

`du -sh *`

即可查看下载的文件大小。也就可以知道是网速不好，还是源不可用了。

解决方案 ：

1、第一次我用淘宝的https://ruby.taobao.org/ 替换自带的软件源，du -sh *查看下载的文件大小，一直不变（不知道是网速的原因，还是淘宝这个源不可用）。然后我就换到这个源https://gems.ruby-china.org/ 。重新搞了一遍。它的下载速度还是蛮快的，文件大约是800多兆。

2、也可以使用cocoapods的镜像索引.

所有项目的Podspec文件都托管在https://github.com/CocoaPods/Specs ,第一次执行pod setup时,CocoaPods会将这些podspec索引文件更新到本地的~/.cocoapods目录下,这个索引文件比较大,所以第一次更新时非常慢.友好人士在国内的服务器建立了Cocoapods索引库的镜像,所以执行索引跟新操作时候会快很多.具体操作方法如下:

	   $ pod repo remove master
	   $ git clone https://github.com/CocoaPods/Specs.git ~/.cocoapods/repos/master
	   $ pod repo update

这是使用gitcafe上的镜像,将以上代码中的 https://gitcafe.com/akuandev/Specs.git 替换成 https://git.oschina.net/akuandev/Specs.git 即可使用oschina上的镜像。


问题四、出现如下

	[!] An error occurred while performing `git pull` on repo `master`.
	[!] /usr/bin/git pull --ff-only

原因： Cocoapods的分支不支持当前最新的Xcode版本

解决办法: 删除master分支 重新建立新的分支

`sudo rm -fr ~/.cocoapods/repos/master`

然后再: pod setup

##### 第五个问题是在别人的博客中看到的

问题五：解决升级EI Capiton CocoaPods "pod: command not found"

升级OS X EI Capiton之后，发现CocoaPods的pod无效了，运行pod后显示："pod: command not found"的错误。

解决步骤：

	1.为了安全起见，执行命令"sudo gem uninstall cocoapods"，卸载原有的CocoaPod
	2.执行命令"sudo gem install -n /usr/local/bin cocoapods"来重新安装cocoapod
	3.如果没有权限执行pod，执行命令"sudo chmod +rx /usr/local/bin/"，赋予/usr/local/bin给予执行与读取权限

原文链接：<http://www.jianshu.com/p/6ff1903c3f11>

### 二、cocoapods的使用

#### 方法一 
--------------------------------------

（1）、查找第三方库

 `pod search AFNetworking`

（2）、创建Podfile文件, 在终端使用cd ＋路径切换到项目所在文件下，然后输入：

`touch Podfile`

就可以在项目目录里看到Podfile文件。也可以使用

`pod init

来创建，

打开Podfile文件：

`open Podfile`

然后 **pod install** 就可以了

（3）、 删除已经配置的类库和移除CocoaPods

可以去查看<http://www.jianshu.com/p/552f21a989ba?utm_source=tuicool&utm_medium=referral> 这篇文章。

#### 方法二
--------------------------------------
1、新建工程,并cd到工程目录

2、新建Podfile文件:vim Podfile

3、按i(英文输入状态下)进入编辑状态

4、输入相应的第三方和版本，比如: (LYJWeiBo 项目名)

	platform:ios,'8.0'
	use_frameworks!
	target 'LYJWeiBo' do
	pod 'AFNetworking', '~> 3.1.0'
	pod 'SnapKit', '~> 3.2.0'
	pod 'SDWebImage', '~> 4.0.0'
	pod 'SVProgressHUD', '~> 2.1.2'
	end

5、编辑好,先按esc,再输入:wq(英文输入状态下)保存退出

6、导入第三方库 `$ pod `

7、需要打开后缀为.xcworkspace的工程文件,以后编码也是在此文件中进行操作。

8、在需要的时候#import "Masnory"就可以使用了。

##### pod install与pod update区别：

1.使用pod install来安装新的库，即使你的工程里面已经有了Podfile，并且已经执行过pod install命令了；所以即使你是添加或移除库，都应该使用pod install。

2.使用pod update [PODNAME] 只有在你需要更新库到更新的版本时候用。

## 小技巧：

最近使用CocoaPods来添加第三方类库，无论是执行pod install还是pod update都卡在了Analyzing dependencies不动原因在于当执行以上两个命令的时候会升级CocoaPods的spec仓库，加一个参数可以省略这一步，然后速度就会提升不少。加参数的命令如下：

	pod install --verbose --no-repo-update 
	pod update --verbose --no-repo-update


​	
参考资料:http://blog.csdn.net/u012960049/article/details/52275272
http://blog.devtang.com/2014/05/25/use-cocoapod-to-manage-ios-lib-dependency/

##cocoapods 升级：
#### 1.首先我们可以查看下当前的版本号命令如下:

	// 可用 "pod --version" 命令查看版本，目前最新版本1.4.0
	JerryMacBook-Pro:~ Jerry.Yao$ pod --version
	1.2.0 // 本机安装的版本
#### 2.在升级之前我们需要了解当前安装的Ruby源地址：

	// 使用命令查看当前使用的是淘宝的源: "gem source -l" 
	JerryMacBook-Pro:~ Jerry.Yao$ gem source -l
	*** CURRENT SOURCES ***
	
	https://ruby.taobao.org/
#### 3.移除淘宝的Ruby源，添加一个新的源（注意：目前淘宝的源已经不能用了）

	// 移除旧的源 命令: "gem sources --remove"
	JerryMacBook-Pro:~ Jerry.Yao$ gem sources --remove https://ruby.taobao.org/
	
	// 添加新的源 命令: "gem sources -a https://gems.ruby-china.org/"
	JerryMacBook-Pro:~ Jerry.Yao$ gem sources -a https://gems.ruby-china.org/	
#### 4.查看新的源是否添加成功，使用的命令和步骤2一样

	// 新的源已经成功添加
	JerryMacBook-Pro:~ Jerry.Yao$ gem source -l
	*** CURRENT SOURCES ***
	
	https://gems.ruby-china.org/

#### 5.开始安装，输入如下命令：

	// 输入命令和电脑开机密码 "sudo gem install cocoapods" 
	JerryMacBook-Pro:~ Jerry.Yao$ sudo gem install cocoapods
	Password:
	Fetching: cocoapods-core-1.3.1.gem (100%)
	Successfully installed cocoapods-core-1.3.1
	Fetching: claide-1.0.2.gem (100%)
	Successfully installed claide-1.0.2
	Fetching: cocoapods-trunk-1.2.0.gem (100%)
	Successfully installed cocoapods-trunk-1.2.0
	Fetching: xcodeproj-1.5.1.gem (100%)
	ERROR:  While executing gem ... (Errno::EPERM)
	    Operation not permitted - /usr/bin/xcodeproj
#### 在这一步有可能会报错”Operation not permitted - /usr/bin/xcodeproj”，解决办法如下

	// 输入命令："sudo gem install -n /usr/local/bin cocoapods -v 1.6.1"
	JerryMacBook-Pro:~ Jerry.Yao$ sudo gem install -n /usr/local/bin cocoapods
	Successfully installed xcodeproj-1.5.1
	Fetching: ruby-macho-1.1.0.gem (100%)
	Successfully installed ruby-macho-1.1.0
	Fetching: cocoapods-1.3.1.gem (100%)
	Successfully installed cocoapods-1.3.1
	Parsing documentation for xcodeproj-1.5.1
	Installing ri documentation for xcodeproj-1.5.1
	Parsing documentation for ruby-macho-1.1.0
	Installing ri documentation for ruby-macho-1.1.0
	Parsing documentation for cocoapods-1.3.1
	Installing ri documentation for cocoapods-1.3.1
	3 gems installed // 安装成功

#### 还有有可能会报错：

	ERROR:  While executing gem ... (TypeError)
	no implicit conversion of nil into String
	
	// 解决办法是执行如下命令更新gem
	sudo gem update --system
####6.再次查看下CocoaPods的版本，已经成功升级咯！

	JerryMacBook-Pro:~ Jerry.Yao$ pod --version
	1.3.1



##cocoapods search 获取不到最新库的解决方法

####1.cocoapods的版本过低 
####2.还没有更新本地仓库

	执行 pod repo update更新本地仓库，本地仓库完成后，即可搜索到指定的第三方库



