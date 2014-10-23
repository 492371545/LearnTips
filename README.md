LearnTips
=========
Dispatch Queues
GCD的基本概念就是dispatch queue。dispatch queue是一个对象，它可以接受任务，并将任务以先到先执行的顺序来执行。dispatch queue可以是并发的或串行的。并发任务会像NSOperationQueue那样基于系统负载来合适地并发进行，串行队列同一时间只执行单一任务。
GCD中有三种队列类型：
	.	The main queue(Main queue): 与主线程功能相同。实际上，提交至main queue的任务会在主线程中执行。main queue可以调用dispatch_get_main_queue()来获得。因为main queue是与主线程相关的，所以这是一个串行队列。
***************************************************************************************************************************************
	.	Global queues(Concurrent queue(global dispatch queue):): 全局队列是并发队列，并由整个进程共享。进程中存在三个全局队列：高、中（默认）、低三个优先级队列。可以调用dispatch_get_global_queue函数传入优先级来访问队列。
	.	可以同时运行多个任务,每个任务的启动时间是按照加入queue的顺序,结束的顺序依赖各自的任务.使用dispatch_get_global_queue获得.
	.	所以我们可以大致了解使用GCD的框架:
	.	dispatch_async(getDataQueue,^{
	.	    //获取数据,获得一组后,刷新UI.
	.	    dispatch_aysnc (mainQueue, ^{
	.	    //UI的更新需在主线程中进行
	.	};
	.	}
	.	)
***************************************************************************************************************************************
	.	用户队列(Serial quque(private dispatch queue)): 用户队列 (GCD并不这样称呼这种队列, 但是没有一个特定的名字来形容这种队列，所以我们称其为用户队列) 是用函数 dispatch_queue_create 创建的队列. 这些队列是串行的。正因为如此，它们可以用来完成同步机制, 有点像传统线程中的mutex。
	.	　每次运行一个任务,可以添加多个,执行次序FIFO. 通常是指程序员生成的,比如:
	.	NSDate *da = [NSDate date];
	.	NSString *daStr = [da description];
	.	const char *queueName = [daStr UTF8String];
	.	dispatch_queue_t myQueue = dispatch_queue_create(queueName, NULL);
==============================================================================================================================================================================================================================================================================

github
使用github管理iOS分布式项目开发 http://www.cnblogs.com/516inc/archive/2012/03/28/2421492.html     （比较详细）
tit /github 使用方法小记：    http://like-eagle.iteye.com/blog/1317009

…or create a new repository on the command line

touch README.md
git init (在初始化远程仓库时最好使用 git --bare init   而不要使用：git init
   如果使用了git init初始化，则远程仓库的目录下，也包含work tree，当本地仓库向远程仓库push时,   如果远程仓库正在push的分支上（如果当时不在push的分支，就没有问题）, 那么push后的结果不会反应在work tree上,  也即在远程仓库的目录下对应的文件还是之前的内容，必须得使用git reset --hard才能看到push后的内容.)
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/492371545/downloadProgress.git  （或：git remote add downloadProgress git@github.com:492371545/downloadProgress.git）
git push -u origin master （git push -u downloadProgress master）
…or push an existing repository from the command line

git remote add origin https://github.com/492371545/downloadProgress.git
git push -u origin master

====================================================================================================================================================
在github上创建仓库：
Create a new repository on the command line


touch README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/BrentHuang/MyRepo.git
git push -u origin master


在本地新建一个分支： git branch Branch1
切换到你的新分支: git checkout Branch1
将新分支发布在github上： git push origin Branch1
在本地删除一个分支： git branch -d Branch1
在github远程端删除一个分支： git push origin :Branch1   (分支名前的冒号代表删除)

直接使用git pull和git push的设置
git branch --set-upstream-to=origin/master master  git branch --set-upstream-to=origin/ThirdParty ThirdParty git config --global push.default matching

==============================================================================================================================================================================================================================================================================
git 上传本地文件到github

1 git config --global user.name "Your Real Name" 
2 git config --global user.email you@email.address

==============================================================================================================================================================================================================================================================================
1.创建一个新的repository：
先在github上创建并写好相关名字，描述。
$cd ~/hello-world        //到hello-world目录
$git init                     //初始化
$git add .                   //把所有文件加入到索引（不想把所有文件加入，可以用gitignore或add 具体文件)
$git commit               //提交到本地仓库，然后会填写更新日志( -m “更新日志”也可)
$git remote add origin git@github.com:WadeLeng/hello-world.git        //增加到remote
$git push origin master    //push到github上
2.更新项目（新加了文件）：
$cd ~/hello-world
$git add .                  //这样可以自动判断新加了哪些文件，或者手动加入文件名字
$git commit              //提交到本地仓库
$git push origin master    //不是新创建的，不用再add 到remote上了
3.更新项目（没新加文件，只有删除或者修改文件）：
$cd ~/hello-world
$git commit -a          //记录删除或修改了哪些文件
$git push origin master  //提交到github
4.忽略一些文件，比如*.o等:
$cd ~/hello-world
$vim .gitignore     //把文件类型加入到.gitignore中，保存
然后就可以git add . 能自动过滤这种文件
5.clone代码到本地：
$git clone git@github.com:WadeLeng/hello-world.git
假如本地已经存在了代码，而仓库里有更新，把更改的合并到本地的项目：
$git fetch origin    //获取远程更新
$git merge origin/master //把更新的内容合并到本地分支
6.撤销
$git reset
7.删除
$git rm  * // 不是用rm
//------------------------------常见错误-----------------------------------
1.$ git remote add origin git@github.com:WadeLeng/hello-world.git

 错误提示：fatal: remote origin already exists.
 解决办法：$ git remote rm origin
 然后在执行：$ git remote add origin git@github.com:WadeLeng/hello-world.git 就不会报错误了
 2. $ git push origin master
 错误提示：error:failed to push som refs to
 解决办法：$ git pull origin master //先把远程服务器github上面的文件拉先来，再push 上去。
====================================================================================================================================================





cocoapods
终端命令：

若镜像太慢则使用淘宝镜像，
1. gem source -l    [输入当前镜像版本信息，若不是http://ruby.taobao.org/则，进行下面2步，若是则跳过]

2. gem source —remove https://rubygems.org/

3. gem source -a http://ruby.taobao.org/

4. sudo gem install cocoapods     [安装cocoapods]

5.在你的工程目录创建Podfile文件用以安装第三方库的脚本

  搜索你要添加库的版本信息：  pod search AFNetworking

6.cd到你项目所在目录，然后在当前目录下，利用vim创建Podfile
   
 vim Podfile

7. 编辑 Podfile,若以下内容不对可按照5中的版本信息修改
  	 platform :ios, '7.0'
	pod "AFNetworking", "~> 2.0"

 按esc再按：wq退出正在编辑的命令。

8.pod install   [下载AFNetworking类库，若卡，可使用pod install --no-repo-update]  

9.pod update   [更新AFNetworking类库，若卡，可使用pod update --no-repo-update]
