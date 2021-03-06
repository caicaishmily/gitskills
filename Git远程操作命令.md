一 git clone

	从远程主机克隆一个版本库
	  1 git clone <版本库的网址>
	   该命令会在本地生成一个目录，与远程主机的版本库同名。如果要指定不同的目录名，
	   可以将目录名作为git clone的第二个参数
	   git clone <版本库的网址>  <本地目录名>
	  2 git clone支持多种协议，除了HTTP(s)以外，还支持SSH、Git、本地文件协议等
	    $ git clone http[s]://example.com/path/to/repo.git/
	    $ git clone ssh://example.com/path/to/repo.git/
	    $ git clone git://example.com/path/to/repo.git/
	    $ git clone /opt/git/project.git
	    $ git clone file:///opt/git/project.git
	    $ git clone ftp[s]://example.com/path/to/repo.git/
	    $ git clone rsync://example.com/path/to/repo.git/

二、git remote


	为了便于管理，Git要求每个远程主机都必须指定一个主机名。git remote命令就用于管理主机名。
		1 不带选项的时候，git remote命令列出所有远程主机。
			git remote
		2 使用-v选项，可以参看远程主机的网址。
			git remote -v
		3 克隆版本库的时候，所使用的远程主机自动被Git命名为origin。如果想用其他的主机名，需要用git clone命令的-o选项指定。
			git clone -o jQuery https://github.com/jquery/jquery.git
		4 git remote show命令加上主机名，可以查看该主机的详细信息
			git remote show <主机名>
		5 git remote add命令用于添加远程主机。
			git remote add <主机名> <网址>
		6 git remote rm命令用于删除远程主机。
			git remote rm <主机名>
		7 git remote rename命令用于远程主机的改名。
			git remote rename <原主机名>  <新主机名>

三 git fetch


	一旦远程主机的版本库有了更新（Git术语叫做commit），需要将这些更新取回本地，这时就要用到git fetch命令。
			1 git fetch <远程主机名>将某个远程主机的更新，全部取回本地
			2 git fetch命令通常用来查看其他人的进程，因为它取回的代码对你本地的开发代码没有影响
		    3 默认情况下，git fetch取回所有分支（branch）的更新。如果只想取回特定分支的更新，可以指定分支名。
				git fetch <远程主机名> <分支名>
				 exm:  git fetch origin master
				所取回的更新，在本地主机上要用"远程主机名/分支名"的形式读取。比如origin主机的master，就要用origin/master读取。
			4  git branch命令的-r选项，可以用来查看远程分支，-a选项查看所有分支。
				git branch -r
				git branch -a
				取回远程主机的更新以后，可以在它的基础上，使用git checkout命令创建一个新的分支。
				git checkout -b newBranch origin/master
			5 使用git merge命令或者git rebase命令，在本地分支上合并远程分支。
				git merge origin/master || git rebase origin/master

四 git pull


	git pull命令的作用是，取回远程主机某个分支的更新，再与本地的指定分支合并。
		1 git pull <远程主机名> <远程分支名>:<本地分支名>
			git pull origin next:master
			上述命令取回origin主机的next分支，与本地的master分支合并
			如果远程分支是与当前分支合并，则冒号后面的部分可以省略，即
			git pull origin next  <=> git fetch origin  git merge origin/next
	  2 git也允许手动建立追踪关系。
			git branch --set-upstream master origin/next
	  3 如果当前分支与远程分支存在追踪关系，git pull就可以省略远程分支名。
			git pull origin
		4 如果当前分支只有一个追踪分支，连远程主机名都可以省略。
			git pull
		5 如果合并需要采用rebase模式，可以使用--rebase选项。
			git pull --rebase <远程主机名> <远程分支名>:<本地分支名>

五 git push


	git push命令用于将本地分支的更新，推送到远程主机
	git push <远程主机名> <本地分支名>:<远程分支名>
	注意，分支推送顺序的写法是<来源地>:<目的地>，所以git pull是<远程分支>:<本地分支>，而git push是<本地分支>:<远程分支>。
	1 如果省略远程分支名，则表示将本地分支推送与之存在"追踪关系"的远程分支（通常两者同名），如果该远程分支不存在，则会被新建
		git push origin master
		上面命令表示，将本地的master分支推送到origin主机的master分支。如果后者不存在，则会被新建。
		如果省略本地分支名，则表示删除指定的远程分支，因为这等同于推送一个空的本地分支到远程分支。

	$ git push origin :master
	# 等同于
	$ git push origin --delete master
	上面命令表示删除origin主机的master分支。
	如果当前分支与远程分支之间存在追踪关系，则本地分支和远程分支都可以省略。

	$ git push origin
	上面命令表示，将当前分支推送到origin主机的对应分支。
	如果当前分支只有一个追踪分支，那么主机名都可以省略。

	$ git push
	如果当前分支与多个主机存在追踪关系，则可以使用-u选项指定一个默认主机，这样后面就可以不加任何参数使用git push。

	$ git push -u origin master
	上面命令将本地的master分支推送到origin主机，同时指定origin为默认主机，后面就可以不加任何参数使用git push了。
	不带任何参数的git push，默认只推送当前分支，这叫做simple方式。此外，还有一种matching方式，会推送所有有对应的远程分支的本地分支。Git 2.0版本之前，默认采用matching方法，现在改为默认采用simple方式。如果要修改这个设置，可以采用git config命令。

	$ git config --global push.default matching
	# 或者
	$ git config --global push.default simple
	还有一种情况，就是不管是否存在对应的远程分支，将本地的所有分支都推送到远程主机，这时需要使用--all选项。

	$ git push --all origin
	上面命令表示，将所有本地分支都推送到origin主机。
	如果远程主机的版本比本地版本更新，推送时Git会报错，要求先在本地做git pull合并差异，然后再推送到远程主机。这时，如果你一定要推送，可以使用--force选项。

	$ git push --force origin
	上面命令使用--force选项，结果导致远程主机上更新的版本被覆盖。除非你很确定要这样做，否则应该尽量避免使用--force选项。
	最后，git push不会推送标签（tag），除非使用--tags选项。

	$ git push origin --tags
