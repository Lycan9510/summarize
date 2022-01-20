### 推荐开发模式

1、在cli_tools进行开发
2、并且写示例代码
3、用示例代码进行调试

### 需要在项目仓库中更新子项目代码

请务必注意，默认跟踪的是master分支。

需要通过下面的代码来切换至dev分支：

```shell
# 执行后，会【仅为你】切换追踪分支
git config submodule.cli_tools.branch dev
git submodule update --remote

# 如果需要将分支切换提交至远程【禁止】，第一条语句修改为
git config -f .gitmodules submodule.cli_tools.branch dev
```

此时git push会将你更新commit ID锁定至远程仓库，如果其他人直接pull代码会更新到你锁定的commit ID。
*生产环境会手动执行一次修改至master，不受影响，测试环境计划支持临时commit ID依赖。*

如需恢复默认跟踪，比如子仓库已经合并至master了：

```shell
# 切换回master分支
git config submodule.cli_tools.branch master
git submodule update --remote

# 可以执行查看当前本地追踪的分支
git config submodule.cli_tools.branch
```

在dev分支下，可以像往常一样提交代码（add commit push等）

<img src="/tfl/pictures/202012/tapd_20382112_1608707395_89.png" width="700px">

如果仅是修改了代码，没有提交子仓库而直接提交了主仓库，子仓库的依赖上会显示为`-dirty`。

![enter image description here](/tfl/pictures/202012/tapd_20382112_1608712999_23.png)

并且其他人无法更新到你的代码，因为只在你本地。


