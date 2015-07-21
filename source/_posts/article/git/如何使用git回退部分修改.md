
categories:
 - git

tags:
 - git

title: 回滚git代码版本
---

**参考**：[Git的撤消操作 - 重置, 签出 和 撤消][^1]

错提交代码，或者某个版本代码之后想要回滚回来的话，就会遇到需要回滚代码版本的问题。

根据代码当前提交状态，回滚操作可以分为二类：
1. 未提交代码
2. 已提交代码


## 回滚未提交代码

[git reset三种方式][^3]：
1. 只回滚本地代码库(HEAD)内容，不回滚索引(index)和工作区(working)内容。
`git reset --soft 版本号`

2. (**默认**)只回滚本地代码库(HEAD)和索引(index)内容，不回滚工作区(working)内容。
`git reset --mixed 版本号`

3. 回滚本地代码库(HEAD)、索引(index)和工作区(working)内容。
`git reset --hard 版本号`

<!-- more -->

**常用命令**

1. 回滚到上一个版本
`git reset HEAD`

2. 回滚到上上个版本
`git reset HEAD^`

3. 回滚到指定版本
`git reset 057d`

**注**：此处回滚的含义是与当前分支远程代码库(target)内容保持一致。

### 举例说明

**参考**：[git-reset][^2]

#### 举例1
| # | working | index | HEAD | target |
|----|----|----|----|----|
| current | A | B | C | D |
| soft | A | B | D | D |
| mixed | A | D | D | D |
| hard | D | D | D | D |

#### 举例2
| # | working | index | HEAD | target |
|----|----|----|----|----|
| current | A | B | C | C |
| soft | A | B | C | C |
| mixed | A | C | C | C |
| hard | C | C | C | C |



## 回滚已提交代码


### 回滚已经提交，且已发布代码

参考：[git revert 用法][^5]

git revert撤销某次操作，此次操作之前和之后的commit和history都会保留，并且把这次撤销作为一次最新的提交。
git revert是提交一个新的版本，将需要revert的版本的内容再反向修改回去，版本会递增，不影响之前提交的内容。

**常用命令**

1. 撤销前一次 commit
`git revert HEAD`

2. 撤销前前一次 commit
`git revert HEAD^`

3. 撤销指定的版本，撤销也会作为一次提交进行保存
`git revert commit fa042ce57ebbe5bb9c8db709f719cec2c58ee7ff`


#### 举例说明
代码版本库中间一段提交内容如下：
```
commit c15ebaadf86bde882035a68e09f0b7832ea271f6
    删除无用文件

commit 763ab06edb1b5054fbd12e94317e2f13ac4b2af5
    修改静态资源版本

commit 277563b24904f3f70eb210f6fa1d3f3f1ee914ac
    pvrs接口切回测试环境

commit a00d98c53e86182054eb4e77a18e3044f5f9d658
    搜索无数据返回，导致空指针异常处理
```

如果期望代码回滚到 **277563b24904f3f70eb210f6fa1d3f3f1ee914ac** 这个状态的话，则需要撤销后一个版本 **763ab06edb1b5054fbd12e94317e2f13ac4b2af5** 以后的所有代码提交。
执行命令：`git revert 763ab06edb1b5054fbd12e94317e2f13ac4b2af5`



### 回滚已经提交，但未发布代码

**参考**：[Git 基础 - 撤消操作][^4]

``` shell
git commit -m 'initial commit'
git add forgotten_file
git commit --amend
```


[^1]: http://gitbook.liuhui998.com/4_9.html
[^2]: https://git-scm.com/docs/git-reset
[^3]: http://blog.csdn.net/wujiangguizhen/article/details/10609647
[^4]: https://git-scm.com/book/zh/v1/Git-%E5%9F%BA%E7%A1%80-%E6%92%A4%E6%B6%88%E6%93%8D%E4%BD%9C
[^5]: http://blog.chinaunix.net/uid-26770731-id-3285880.html