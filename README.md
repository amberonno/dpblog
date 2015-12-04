# dpblog
企业平台-前端技术文档

撰写方式
========
1. fork 此项目,拉自己的fork到本地。
2. [hexo](https://hexo.io/zh-cn/)为博客框架。用hexo new {title}建立自己的文章。
3. 建立好的文章在./sorce/_posts/{title}.md，用[markdown](http://www.jianshu.com/p/1e402922ee32/)语法进行撰写。
4. 写好以后运行hexo generate生成静态页面，hexo server运行本地服务器，打开localhost:4000/查看自己撰写的博客。
5. git将修改push到自己的fork,然后pull request进行提交修改。
6. 我会以天的单位发布撰写好的文档。

更新 主干代码

# Add the remote, call it "upstream":

git remote add upstream https://github.com/whoever/whatever.git

# Fetch all the branches of that remote into remote-tracking branches,
# such as upstream/master:

git fetch upstream

# Make sure that you're on your master branch:

git checkout master

# Rewrite your master branch so that any commits of yours that
# aren't already in upstream/master are replayed on top of that
# other branch:

git rebase upstream/master

If you don't want to rewrite the history of your master branch, (for example because other people may have cloned it) then you should replace the last command with git merge upstream/master. However, for making further pull requests that are as clean as possible, it's probably better to rebase.

Update: If you've rebased your branch onto upstream/master you may need to force the push in order to push it to your own forked repository on GitHub. You'd do that with:

git push -f origin master
You only need to use the -f the first time after you've rebased.
