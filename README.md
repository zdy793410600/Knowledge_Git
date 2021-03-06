## Knowledge_Git
本篇笔记是根据在Github上的一个Git使用教程总结的各种命令及使用方法[learnGitBranching](https://github.com/pcottle/learnGitBranching)

### 1、本地提交 - commit
Git 仓库中的提交记录保存的是你的目录下所有文件的快照，就像是把整个目录复制，然后再粘贴一样，但比复制粘贴优雅许多！

Git 希望提交记录尽可能地轻量，因此在你每次进行提交时，它并不会盲目地复制整个目录。条件允许的情况下，它会将当前版本与仓库中的上一个版本进行对比，并把所有的差异打包到一起作为一个提交记录。

Git 还保存了提交的历史记录。这也是为什么大多数提交记录的上面都有父节点的原因 —— 我们会在图示中用箭头来表示这种关系。对于项目组的成员来说，维护提交历史对大家都有好处。

关于提交记录太深入的东西咱们就不再继续探讨了，现在你可以把提交记录看作是项目的快照。提交记录非常轻量，可以快速地在这些提交记录之间切换！

* 将代码提交到本地
```
git commit 
```
* 将代码提交到本地并添加描述信息
```
git commit -m "我是描述信息"
```

### 2、查看/创建分支 - branch
Git 的分支也非常轻量。它们只是简单地指向某个提交纪录 —— 仅此而已。所以许多 Git 爱好者传颂：

早建分支！多用分支！

这是因为即使创建再多分的支也不会造成储存或内存上的开销，并且按逻辑分解工作到不同的分支要比维护那些特别臃肿的分支简单多了。

在将分支和提交记录结合起来后，我们会看到两者如何协作。现在只要记住使用分支其实就相当于在说：“我想基于这个提交以及它所有的父提交进行新的工作。”

* 查看分支
列出本地已经存在的分支，并且在当前分支的前面用"\*"标记。
```
git branch
```
* 查看本地分支
```
git branch -l
```
* 查看远程分支
```
git branch -r
```
* 查看全部分支
```
git branch -a
```
* 创建本地分支（仅仅是创建新分支，创建完之后还会处于当前分支，并不会切换到新分支上去）
```
git branch <要创建的新分支的名称>
```
* 切换分支
```
git checkout <分支名称>
```
* 创建并切换到新分支
```
git checkout -b <要创建的新分支的名称>
```

### 3、分支与合并 - merge
太好了! 我们已经知道如何提交以及如何使用分支了。接下来咱们看看如何将两个分支合并到一起。就是说我们新建一个分支，在其上开发某个新功能，开发完成后再合并回主线。

咱们先来看一下第一种方法 —— git merge。在 Git 中合并两个分支时会产生一个特殊的提交记录，它有两个父节点。翻译成自然语言相当于：“我要把这两个父节点本身及它们所有的祖先都包含进来。”

通过图示更容易理解一些，咱们到下一页看一下。

* merge 合并分支
首先查看你现在所属的分支，然后通过命令将你想要合并的分支合并到当前分支。
```
git merge <想要合并到当前分支的其他分支的名称>
```

### 4、分支与合并 - rebase
第二种合并分支的方法是 git rebase。Rebase 实际上就是取出一系列的提交记录，“复制”它们，然后在另外一个地方逐个的放下去。

Rebase 的优势就是可以创造更线性的提交历史，这听上去有些难以理解。如果只允许使用 Rebase 的话，代码库的提交历史将会变得异常清晰。

咱们还是实际操作一下吧……

* rebase 合并分支
合并其他分支到当前分支，并且能够保证一个串型的提交记录。
```
git rebase <想要合并到当前分支的其他分支的名称>
```

### 5、分离HEAD - 在提交树上移动
在接触 Git 更高级功能之前，我们有必要先学习在你项目的提交树上前后移动的几种方法。

一旦熟悉了如何在 Git 提交树上移动，你驾驭其它命令的能力也将水涨船高！

我们首先看一下 “HEAD”。 HEAD 是一个对当前检出记录的符号引用 —— 也就是指向你正在其基础上进行工作的提交记录。

HEAD 总是指向当前分支上最近一次提交记录。大多数修改提交树的 Git 命令都是从改变 HEAD 的指向开始的。

HEAD 通常情况下是指向分支名的（如 bugFix）。在你提交时，改变了 bugFix 的状态，这一变化通过 HEAD 变得可见。
![image](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head01.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head02.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head03.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head04.png)


* 将HEAD指向某次提交记录
```
git checkout <某次提交的记录>
```

### 6、分离HEAD - 相对引用
通过指定提交记录哈希值的方式在 Git 中移动不太方便。在实际应用时，并没有像本程序中这么漂亮的可视化提交树供你参考，所以你就不得不用 git log 来查查看提交记录的哈希值。

并且哈希值在真实的 Git 世界中也会更长（译者注：基于 SHA-1，共 40 位）。例如前一关的介绍中的提交记录的哈希值可能是 fed2da64c0efc5293610bdd892f82a58e8cbc5d8。舌头都快打结了吧...

比较令人欣慰的是，Git 对哈希的处理很智能。你只需要提供能够唯一标识提交记录的前几个字符即可。因此我可以仅输入fed2 而不是上面的一长串字符。

正如我前面所说，通过哈希值指定提交记录很不方便，所以 Git 引入了相对引用。这个就很厉害了!

使用相对引用的话，你就可以从一个易于记忆的地方（比如 bugFix 分支或 HEAD）开始计算。

相对引用非常给力，这里我介绍两个简单的用法：

使用 ^ 向上移动 1 个提交记录  
使用 ~<num> 向上移动多个提交记录，如 ~3  
  
#### 6.1、分离HEAD - "^"操作符
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head05.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head06.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head07.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head08.png)  

* 将HEAD指定到某次提交的上一级
```
git checkout <某次提交的记录>^
```
* 将HEAD指定到当前HEAD指向位置的上一级
```
git checkout HEAD^
```

#### 6.2、分离HEAD - "~"操作符
如果你想在提交树中向上移动很多步的话，敲那么多 ^ 貌似也挺烦人的，Git 当然也考虑到了这一点，于是又引入了操作符 ~。

该操作符后面可以跟一个数字（可选，不跟数字时与 ^ 相同，向上移动一次），指定向上移动多少次。咱们还是通过实际操作看一下吧

![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head09.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head10.png)  

* 将HEAD指向某次提交指定<num>次的之前的位置

```
git checkout <某次提交记录> ~<num>  
```
* 将HEAD指向当前HEAD指定<num>次的之前的位置

```
git checkout HEAD ~<num>  
```

#### 6.3、分离HEAD - 强制修改分支位置
你现在是相对引用的专家了，现在用它来做点实际事情。

我使用相对引用最多的就是移动分支。可以直接使用 -f 选项让分支指向另一个提交。例如:
```
git branch -f master HEAD~3
```
上面的命令会将 master 分支强制指向 HEAD 的第 3 级父提交。

![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head11.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_checkout_head12.png)  

### 7、撤销变更
在 Git 里撤销变更的方法很多。和提交一样，撤销变更由底层部分（暂存区的独立文件或者片段）和上层部分（变更到底是通过哪种方式被撤销的）组成。我们这个应用主要关注的是后者。

主要有两种方法用来撤销变更 —— 一是 git reset，还有就是 git revert。接下来咱们逐个进行讲解。

#### 7.1、撤销变更 - reset
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_reset_head01.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_reset_head02.png)  

* 撤销当前HEAD的上<num>次提交  
  
```
git reset HEAD~<num>
```

#### 7.2、撤销变更 - revert
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_revert_head01.png)
![iamge](https://raw.githubusercontent.com/zdy793410600/Knowledge_Git/master/Git_HEAD/git_revert_head02.png)  

* 撤销指定某次提交
```
git revert <某次提交记录>
```
