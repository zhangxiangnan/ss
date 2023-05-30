#### .gitignore文件不生效（.gitignore is not work）
git rm -r --cached .

#### 拉取
- git pull：拉取当前分支的最新代码
- git pull origin master：拉取远端master代码到当前分支
- git pull origin xx：拉取远端xx分支代码到当前分支

#### 推送
- git push：推送当前分支代码到远端同样分支

#### 合并冲突
- git merge --abort：合并中途撤销、还原合并

#### Git栈（本地,类似idea的Shelve）
- git stash：隐藏变更
- git stash pop：显示变更

#### 推送新建分支与远端关联
git push --set-upstream origin xx-cur-branch-name

