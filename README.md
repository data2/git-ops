# git-ops

# 脚本来改变某个repo的Git历史
我们已经创建了一个脚本，使用正确的姓名和电子邮件地址提交后，你以前提交的所有的commits中的作者信息及提交者字段中的旧的用户名和邮箱地址都将被更正
注意： 执行这段脚本会重写 repo 所有协作者的历史。完成以下操作后，任何 fork 或 clone 的人必须获取重写后的历史并把所有本地修改 rebase 入重写后的历史中。
在执行这段脚本前，你需要准备的信息：
Mac、Linux下打开Terminal，Windows下打开命令提示符（command prompt）
给你的repo创建一个全新的clone
```
git clone --bare https://github.com/user/repo.git
cd repo.git
```
复制粘贴脚本，并根据你的信息修改以下变量：旧的Email地址，正确的用户名，正确的邮件地址
```shell
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="旧的Email地址"
CORRECT_NAME="正确的用户名"
CORRECT_EMAIL="正确的邮件地址"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```

执行脚本。

用git log命令看看新 Git 历史有没有错误

把正确历史 push 到 Github

```git push --force --tags origin 'refs/heads/*'```

删掉刚刚临时创建的 clone

```
cd ..
rm -rf repo.git
```
