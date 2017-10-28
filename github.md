#1、URL
	* https://github.com

#2、创建本地ssh key
	* 命令
		* ssh-keygen -t rsa -C "you_email@your_email.com" ("your_email" is that email you regist on github)
	* 测试是否设置成功
		* ssh -T git@github.com
	
#3、设置用户名和邮箱,每次提交时github会记录
	* git config --global user.name "your name"
	* git config --global user.email "your email"

#4、添加远程地址
	* git remote add origin git@github.com:youName/youRepo.git
	* youName为你的github的用户名，youRepo为github上的git仓库

#5、
	* 提交
		* git add <filename>
		* git add * 
		* git rm <filename>
		* git commit -m "提交信息"
		* git push origin master (同步到线上)
	* 更新
		* git pull

	* 回滚
		* git checkout -- <filename>
		* git checkout -- * 
		* git reset --hard origin/master
	* 查看
		* git status
		* git log
		* git diff
	* 分支
		* git checkout -b feature 创建分支(feature为分支名称)
		* git checkout master 切换分支
		* git branch -D feature 删除分支
		* git branch 查看分支
	* 克隆
		* git clone https://github.com/ls200906371/studyNotes.git