## Git学习



#### git submodule

使用背景：开发过程中，经常会有一些通用的部分希望抽取出来做成一个公共库来提供给别的工程来使用，而公共代码库的版本管理是个麻烦的事情

（1）【为当前工程添加submodule】，命令：git submodule add 仓库地址  路径

（2）【下载的工程带有submodule】

当使用git clone下来的工程中带有submodule时，初始的时候，submodule的内容并不会自动下载下来的，此时，只需执行如下命令：

git submodule update --init  --recursive

即可将子模块内容下载下来后工程才不会缺少相应的文件



#### git reflog 

可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）



#### git commit --amend

git commit -m 提交之后，发现-m的说明文字写的有问题，想要重新写一次，也就是想撤销上次的提交动作，重新提交一次



#### git grep <关键词>

搜索含有关键词的文件



#### git brame  <文件名路径>

查看指定文件每一行的提交人和提交时间



#### git log -p  <文件名路径>

查看指定文件的每一次提交和改动



#### git rebase

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220503102004401.png" alt="image-20220503102004401" style="zoom: 33%;" />

<img src="/Users/anrui/Library/Application Support/typora-user-images/image-20220503102154321.png" alt="image-20220503102154321" style="zoom:18%;" />

 

git rebase与git merge的区别

git merge

- 合并后根据时间排序版本节点，会新生成merge版本节点
- 默认情况下，当前分支如果落后于对方分支，如果合并的话则不会生成新的merge节点
- 使用-no-ff这个参数，则一定会新生成merge版本节点

git rebase

- 会保持自己的版本顺序，不会新生成merge版本节点

- 冲突的解决

  git add后不要用git commit，用git rebase --continue