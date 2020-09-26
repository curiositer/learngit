### 初始化

使用`git init`命令。



### 添加文件到Git仓库

分两步：

1. 使用命令`git add <file>`，注意，可反复多次使用，添加多个文件；
2. 使用命令`git commit -m <message>`，完成。



### 查看工作区状态或修改内容

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。



### 更改当前版本（回退版本）

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。



### add、commit区别

多次修改后，如果不用`git add`到暂存区，那就不会加入到`commit`中。 即commit只提交进行过add的内容



### 撤销暂存或丢弃修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考[版本回退](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)一节，不过前提是没有推送到远程库。



### 删除版本库中文件

命令`git rm`用于删除一个文件。如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失**最近一次提交后你修改的内容**。 



### 远程仓库（github）

#### 推送
要关联一个远程库，使用命令`git remote add origin git@server-name:path/repo-name.git`；

关联后，使用命令`git push -u origin master`第一次推送master分支的所有内容；

此后，每次本地提交后，只要有必要，就可以使用命令`git push origin master`推送最新修改；

#### 克隆

要克隆一个仓库，首先必须知道仓库的地址，然后使用`git clone`命令克隆。

Git支持多种协议，包括`https`，但通过`ssh`支持的原生`git`协议速度最快。



### 分支

Git鼓励大量使用分支：

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

#### 详细使用

1. `git log --oneline --decorate`查看各个分支当前所指的对象
2. `git log --oneline --decorate --graph --all`可查看分叉历史。
   
   输出如下：

    ```
    $ git log --oneline --decorate --graph --all
    * c2b9e (HEAD, master) made other changes
    | * 87ab2 (testing) made a change
    |/
    * f30ab add feature #32 - ability to add new formats to the
    * 34ac2 fixed bug #1328 - stack overflow under certain conditions
    * 98ca9 initial commit of my project
    ```

#### 分支使用过程

1. 确保在做分支操作前，当前分支(iss53)保持好一个干净的状态。
2. 切换回主分支`git checkout master`
3. 创建一个新分支(并切换)来完成紧急的任务`git checkout -b hotfix`
4. 紧急任务完成后，切换回主分支，将该修改合并应用
   ```
   git checkout master
   git merge hotfix
   ```
5. 完成修改后，新建的hotfix 分支应该删除`git branch -d hotfix`因为已经不再需要它了 —— 它与master 分支已经指向了同一个位置。
6. 紧急任务已完成，切换回原分支继续之前的开发`git checkout iss53`

> 分支合并如遇到问题，可参考[该网站](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%88%86%E6%94%AF%E7%9A%84%E6%96%B0%E5%BB%BA%E4%B8%8E%E5%90%88%E5%B9%B6)在"遇到冲突时的分支合并"部分寻找解决方案