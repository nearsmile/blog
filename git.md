# git

## 命令列表

* git fsck --lost-found 
  * 找回git add过但是已经不存在文件中的内容
* find .git/objects -type f | xargs ls -lt | sed 60q
  * 这个里面的60参数可以更改成任意你喜欢的数字，比如250啦，它只是代表你想找回的最近的多少次add过的文件
  * .git/lost-found/other这个路径，你可以恢复任何你git add过的文件！再通过find .git/objects -type f | xargs ls -lt | sed 60q这个命令，你就可以找到最近被你add到本地仓库的60个文件

## 提醒

* 没事不要随便用  git reset --hard xxxxxx 命令，这个强制恢复到某次提交