# git学习

## 1.初始化配置

配置本机器的上传时用户名（和能否上传没有关系，验证是跟这个无关的，就是告诉仓库的所有者们是谁上传了

```
git config --global user.name "liuyifan"//设置用户名
git config user.name //查看用户名
```

配置上传时的邮箱（随便配置一个邮箱即可

```
git config --global user.email 853167728@qq.com
git config user.email //查看
```

保存所配置的用户名以及邮箱

```
git config --global credential.helper store
```

查看所配置的信息

```
git config --list
```

==在 Git 和 Gitee（或 GitHub、GitLab）中，**凭据（用户名 + 访问令牌/密码）** 和 **Git 配置的用户名 & 邮箱** 是两回事，分别用于不同的目的。==

------

补充**1️⃣ 为什么 Git 需要凭据？**

Git 需要凭据是为了**访问远程仓库（如 Gitee）时进行身份验证**，以确保你有权限执行 `git push`、`git pull` 等操作。

当你使用 **HTTPS 方式** 连接 Gitee 时：

- **用户名**：你的 Gitee 账号（不带 `@`）。
- **密码**：你的 Gitee 个人访问令牌（PAT）。

如果没有凭据，Git 无法确认你的身份，就会要求你输入用户名和密码（PAT）。

**避免每次输入密码的方法：**

- 使用 **Windows 凭据管理器** 存储凭据（推荐）。
- 使用 SSH 免密认证（更安全）。

------

**2️⃣ Git 配置的用户名和邮箱是什么？**

当你使用 Git 时，每次提交（commit）都会记录**是谁提交的**。这个信息存储在 Git 的本地配置中，通过以下命令设置：

```
sh复制编辑git config --global user.name "你的名字"
git config --global user.email "你的邮箱"
```

**这些信息的作用：**

✅ 仅用于 **本地提交记录**，不会影响身份验证。
✅ 其他开发者可以在 `git log` 看到是谁提交了代码。
✅ 这个邮箱应该和 Gitee 账号一致，以便 Gitee 识别你的提交。

**查看当前配置：**

```
sh


复制编辑
git config --global --list
```

------

**3️⃣ 二者的区别总结**

| 作用         | 需要配置的内容                 | 用于哪里                | 备注                           |
| ------------ | ------------------------------ | ----------------------- | ------------------------------ |
| **身份验证** | Gitee 用户名 + 访问令牌（PAT） | `git push` / `git pull` | 用于访问远程仓库               |
| **提交记录** | Git 用户名 + 邮箱              | `git commit`            | 记录提交者信息，不影响远程认证 |

------

**🔹 什么时候需要配置？**

- **刚安装 Git** 时，需要配置 `user.name` 和 `user.email`。
- **第一次 push 代码** 到远程仓库时，Git 会要求输入 Gitee 凭据（可以保存）。
- 如果使用 **SSH 认证**，就不需要每次输入 Gitee 凭据了。

---

==总结==：1.配置本机器的用户名其实就是告诉仓库谁上传了，和身份验证没有关系，所以用户名和邮箱都可以随便填。有一点需要注意记住就是我们所用的凭据也好还是访问令牌（Personal Access Token, PAT）（**就是一种更安全的东西，用来代替密码**），都是相当于身份验证，我们使用哪个账号去做验证，比如我有个私人仓库，而我在本机器使用的令牌还有凭据如果不是仓库拥有者的用户名和密码就会不通过，导致不能上传和拉取。

2. 还有就是我们需要ssh秘钥就是使用ssh协议拉取上传代码，本质上也是和上面两种一类东西，就是身份验证的。

3. 区别：

   **凭证**和**令牌**更常见于HTTPS协议，它们通常用于简单的身份验证，尤其是在没有设置SSH密钥时。

   **SSH密钥**则用于SSH协议，通过密钥对进行认证，适合更高安全性或自动化操作的场景。

4. 公钥和秘钥的联系和区别：

   1. 秘钥：
      - 这是唯一的私有的钥匙
      - 在电脑上运行ssh-keygn命令生成
      - 它存放在电脑上非常安全的位置，通常在用户目录下的.ssh/id_rsa文件
      - 作用：当你想通过ssh访问github时，你的本地git客户端会用这个秘钥来证明你是你
   2. 公钥：
      - 这是公开安装在锁上的部分
      - 你生成私钥的同时会自动生成它：通常在.ssh/id_rsa.pub
      - 你得分发公钥：这不是秘密，我们需要把公钥赋值出来主动添加到github上

5. 不管是拉取还是上传代码通过ssh协议都需要秘钥和公钥。

## 2.新建仓库

windows下需要打开gitbash,然后再git bash里进行一系列操作

在所需要创造仓库的文件夹下面直接使用（一般是远程仓库创建好直接就clone下来，这样就少了不少操作

```
git init
```

就会出现.git的隐藏文件夹

但是此时的本地仓库是没有和远程的仓库进行关联的所以我们还要在gitee上创造一个仓库进行关联，但是不要勾选初始化仓库

这个时候还需要在windows上设置凭证不然需要一直输入用户名以及密码

---

然后还需要创造README.md文件

`touch README.md` 这个命令的作用是在当前目录下创建一个名为 `README.md` 的文件。如果该文件已存在，则 `touch` 只会更新它的修改时间，而不会改变文件内容。

**README.md 的用途**

`README.md` 是一个用于存放项目信息的 Markdown 文件，通常包含：

- 项目的介绍
- 如何安装和使用
- 依赖项
- 贡献指南
- 许可证信息

很多代码托管平台（如 GitHub、GitLab、Gitee）会自动渲染 `README.md` 并在仓库主页上显示内容。

**示例**

如果你刚初始化了 Git 仓库，创建 `README.md` 并添加一些信息：

```
touch README.md
echo "# 我的项目" >> README.md
git add README.md
git commit -m "添加 README 文件"
git remote add origin https://gitee.com/your-username/repo-name.git
git push origin main//这样就完成了仓库的创建
```

这样，远程仓库就会显示 `README.md` 的内容了。

---

本地 Git 仓库初始化 (`git init`) 之后，你需要**手动**与远程仓库（如 Gitee、GitHub、GitLab）建立联系。以下是完整的操作步骤：

------

**1️⃣ 在 Gitee 创建远程仓库**

1. **登录 Gitee**（https://gitee.com）。
2. 点击右上角 **“+” → “新建仓库”**。
3. 填写 **仓库名称**，然后点击 **创建**。
4. 复制仓库的 **HTTPS 或 SSH 地址**（例如 `https://gitee.com/your-username/repo-name.git`）。

------

**2️⃣ 在本地初始化仓库并建立联系**

如果你已经有一个本地 Git 仓库（`git init` 过），执行以下操作：

**方式 1：使用 HTTPS 方式（推荐）**

```
#进入你的本地仓库目录
cd /path/to/your-repo

# 关联远程仓库（替换为你的 Gitee 仓库地址）
git remote add origin https://gitee.com/your-username/repo-name.git

# 确保关联成功
git remote -v
```

🔹 **使用 HTTPS 时**，Gitee 可能要求你输入 **用户名和个人访问令牌（PAT）** 进行身份验证。

<img src="images\image-20250206022829305.png" alt="image-20250206022829305" />

## 3.添加和提交文件

想要添加和提交文件，我们首先得知道仓库里有什么文件

==查看本地仓库里的文件的命令==

```
git ls-files
```

`git ls-files` 命令用于列出当前 Git 仓库中的所有已跟踪文件（即 Git 正在跟踪并已经被添加到暂存区或已提交的文件）。它不会列出未被 Git 跟踪的文件。

基本用法

```
bash


复制编辑
git ls-files
```

这将列出当前工作目录下所有已跟踪的文件。输出的结果是按路径列出的文件名。

常用选项

1. **列出已修改但未暂存的文件**：

   ```
   bash
   
   
   复制编辑
   git ls-files -m
   ```

   这个选项会列出所有已修改但尚未添加到暂存区（`git add`）的文件。

2. **列出已暂存的文件**：

   ```
   bash
   
   
   复制编辑
   git ls-files -s
   ```

   这个选项会列出所有已暂存的文件，并显示它们的 Git 状态（如版本、文件哈希等）。

3. **列出所有文件，包括未跟踪的文件**：

   ```
   bash
   
   
   复制编辑
   git ls-files --others
   ```

   这个选项列出所有未被 Git 跟踪的文件（即那些没有添加到 Git 的文件）。

4. **列出已忽略的文件**：

   ```
   bash
   
   
   复制编辑
   git ls-files --ignored
   ```

   这个选项列出所有被 `.gitignore` 忽略的文件。

5. **列出特定路径的文件**：

   ```
   bash
   
   
   复制编辑
   git ls-files <path>
   ```

   你可以传递路径参数，只列出该路径下的已跟踪文件。

   

查看本地文件状态,处于哪个分支以及commit了什么

```
git status
```

没有被跟踪意思就是没有被add的文件

假如我们已经把更改的文件add到暂存区的时候，我们可以用两个操作把跟踪的文件还原到未add的状态

**仅从 Git 追踪中移除，但保留本地文件**：

```
sh


复制编辑
git rm --cached filename
```

适用于希望文件不再受 Git 版本控制但仍保留在本地的情况，比如 `.gitignore` 之前已经提交的文件。

==还有一种办法就是==

1. **撤回单个文件的暂存状态**

```
sh


复制编辑
git reset HEAD filename
```

这个命令会把 `filename` 从暂存区移除，但不会改变工作区（working directory）的内容，文件的修改仍然存在。

2. **撤回所有文件的暂存状态**

```
sh


复制编辑
git reset HEAD
```

这会把所有已 `git add` 的文件从暂存区移除，但不会影响实际修改。

3. **撤销 `git add .`（所有文件）**

如果你不小心 `git add .` 添加了所有改动，可以使用：

```
sh


复制编辑
git reset
```

或者：

```
sh


复制编辑
git reset HEAD .
```

这会撤回所有暂存的改动，但文件的修改仍然存在。

```
git log --online //简洁的查看版本的提交信息
```

## 4.git reset

<img src="images\image-20250206221650917.png" alt="image-20250206221650917" />

**总结**

| 命令                               | 作用                           | 影响暂存区   | 影响工作区 |
| ---------------------------------- | ------------------------------ | ------------ | ---------- |
| `git reset HEAD^` (默认 `--mixed`) | 撤销最近一次提交，保留改动     | ❌ 移除暂存区 | ✅ 保留     |
| `git reset --soft HEAD^`           | 撤销最近一次提交               | ✅ 保持暂存区 | ✅ 保留     |
| `git reset --hard HEAD^`           | 撤销最近一次提交，丢弃所有更改 | ❌ 清空       | ❌ 清空     |

## 5.使用gitdiff查看差异

`git diff` 是 Git 用于比较文件差异的命令，可以用于查看未提交的更改、暂存区和历史提交之间的差异。以下是它的常见用法：

------

**1. 查看工作区与暂存区的差异**

```
sh


复制编辑
git diff
```

- 显示**工作区**（working directory）中未暂存的更改（即 `git add` 之前的更改）。
- 如果你修改了一个文件但还没 `git add`，可以用这个命令查看修改的内容。

📌 **示例**

```
sh


复制编辑
git diff
```

输出：

```
diff复制编辑diff --git a/file.txt b/file.txt
index e69de29..d95f3ad 100644
--- a/file.txt
+++ b/file.txt
@@ -0,0 +1,2 @@
+Hello World
+This is a new line
```

表示 `file.txt` 添加了两行内容。

------

**2. 查看暂存区与上次提交的差异**

```
sh


复制编辑
git diff --staged
```

或：

```
sh


复制编辑
git diff --cached
```

- 这个命令会显示**已 `git add` 但还未 `git commit` 的更改**。
- 适用于查看哪些更改即将被提交。

📌 **示例**

```
sh复制编辑git add file.txt
git diff --staged
```

这样你可以在提交前检查自己暂存的改动。

------

**3. 查看最近一次提交与工作区的差异**

```
sh


复制编辑
git diff HEAD
```

- 这个命令会显示**当前工作区的修改**，无论是否 `git add` 了，都能看到。
- 它相当于 `git diff` + `git diff --staged`。

------

**4. 比较两个不同的提交**

```
sh


复制编辑
git diff commit1 commit2
```

- 显示 `commit1` 和 `commit2` 之间的差异。

📌 **示例**

```
sh


复制编辑
git diff 7e4a5b2 3f2c1d9
```

比较 `7e4a5b2` 和 `3f2c1d9` 两个提交的内容差异。

------

**5. 查看特定文件的差异**

```
sh


复制编辑
git diff filename
```

- 只查看某个文件的修改。

📌 **示例**

```
sh


复制编辑
git diff file.txt
```

仅查看 `file.txt` 的差异。

------

**6. 查看某个提交的更改**

```
sh


复制编辑
git diff commit_id
```

- 显示指定 `commit_id` 与其前一个提交之间的差异。

📌 **示例**

```
sh


复制编辑
git diff 7e4a5b2
```

显示 `7e4a5b2` 这个提交与它的前一个提交的不同。

------

**7. 显示提交的完整变更**

如果你想查看某个提交的所有更改（而不是与上一个提交的对比），用：

```
sh


复制编辑
git show commit_id
```

📌 **示例**

```
sh


复制编辑
git show 7e4a5b2
```

会显示该提交的修改详情。

------

**总结**

| 命令                                      | 作用                                               |
| ----------------------------------------- | -------------------------------------------------- |
| `git diff`（已存在文件的修改              | 查看工作区与暂存区的差异（未 `git add` 的更改）    |
| `git diff --staged` / `git diff --cached` | 查看暂存区与上次提交的差异（已 `git add` 的更改）  |
| `git diff HEAD`                           | 查看最近一次提交与工作区的差异（所有未提交的更改） |
| `git diff commit1 commit2`                | 比较两个提交的差异                                 |
| `git diff filename`                       | 查看指定文件的修改                                 |
| `git diff commit_id`                      | 查看某个提交与前一个提交的差异                     |
| `git show commit_id`                      | 显示某个提交的完整修改记录                         |

## 6.git rm

`git rm --cached <file>` **只会删除暂存区（index）中的文件，不会删除工作目录中的文件**。

**具体行为**

- **文件仍然存在于本地（工作目录）**，但 Git 不再跟踪它（如果 `.gitignore` 中有相应规则，Git 之后也不会再跟踪）。
- **不会影响远程仓库**，但如果你提交并推送后，远程仓库会删除该文件。

**示例**

**1. 假设你已经提交了 `config.json`，但想让 Git 忽略它**

```
bash复制编辑echo "config.json" >> .gitignore  # 添加到 .gitignore
git rm --cached config.json       # 从 Git 暂存区移除
git commit -m "Stop tracking config.json"
git push origin main              # 推送更改
```

此时：

- `config.json` 仍然在你的本地目录中。
- Git 认为 `config.json` 被删除，提交后远程仓库中的 `config.json` 会被删除。
- 由于 `.gitignore` 规则，Git 之后不会再跟踪 `config.json`。

**2. 如果你希望删除本地文件并停止跟踪**

```
bash复制编辑git rm config.json  # 这样会同时删除暂存区和本地文件
git commit -m "Remove config.json"
git push origin main
```

**区别**：

- `git rm --cached` ✅ **保留本地文件**，但 Git 不再跟踪它。
- `git rm` ❌ **删除本地文件**，同时从 Git 仓库中删除。



## 7. .ignore

### 7.1使用方法

如何忽略变化的文件，把文件名写入到.ignore中

<img src="images\image-20250210235954650.png" alt="image-20250210235954650" />

写入文件的格式还可以加入通配符

```
*.log
```

==有个值得注意的地方：我们只能忽略不在版本库里的文件，如果已经在版本库里得删除该文件才可以生效==

如果要忽略整个目得在文件名的后面加上/

7.2匹配原则

从上到下每行

<img src="images\image-20250211001712704.png" alt="image-20250211001712704" />

<img src="images\image-20250211001737915.png" alt="image-20250211001737915" />

## 8.ssh配置远程仓库和克隆仓库（github

配置ssh秘钥
1.首先得进入到用户根目录2.然后进入到.ssh文件夹下3.使用这个命令来生成秘钥

```
ssh-keygen -t rsa -b 4096 //rsa是协议 4096是大小
```

passphares 是保护秘钥的密码短语 Lyf128544562

没有任何扩展名的就是私钥文件，有pub结尾的就是公钥文件

打开公钥文件，然后复制公钥文件的内容然后到仓库拥有者的创造ssh key的地方去粘贴上去

创建好秘钥之后，假如==不是第一次创建秘钥还需要添加一步：==就是修改git查找秘钥的路径就是先创建一个config
然后得在config里插入一下文字

```
# github
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/test
```

然后就可以了

## 9.关联本地仓库和远程仓库

首先本地仓库得

```
git init
git remote add origin git@github.com:Lyf-liuyifan/third-repo.git//例子
git push -u origin master:master
```

拉取最新代码

```
git pull origin branch //branch为分支名
```

<img src="images\image-20250217220202369.png" alt="image-20250217220202369" />

## 10.GUI工具（git的可视化工具

SourceTree和gitcraken都可以拿来使用

## 11.vscode中使用git

在git bash中我们可以使用git . 的命令来打开vscode当前命令

然后我们如果修改相关文件可以在<img src="images\image-20250219225539461.png" alt="image-20250219225539461" />查看，这里就等于git里的工作区、暂存区还有本地仓库

具体操作<img src="images\Snipaste_2025-02-19_23-11-07.png" style="zoom:50%;" />

## 12.分支简介和基本操作

创建新分支

```
git branch + 分支名	
```

切换分支

```
git switch + 分支名
```

git checkout 更多现在是用于恢复文件的作用、

合并分支
首先我们要切换到目标分支：比如我们需要把dev分支合并到main分支上首先我们得切换到main分支然后merge过去

```
git merge dev main
```

合并之后会自动commit一次，我们需要输入提交信息

假如是合并之后我们可以

```
git branch -b 分支名 //来强制删除这个分支
```

假如没有合并我们只能在后面加上-B来删除

## 13.合并分支解决冲突

当文件冲突时，我们可以用git status 或者git diff命令来查看冲突文件或内容（得在使用merge命令之后才可以
然后我们需要打开冲突文件去修改文件，两个文件的内容都会保留到合并的分支的该文件中

如果想要中途中止合并的过程我们需要使用这个命令

```
git merge --abort
```

## 14.回退和rebase

<img src="images\Snipaste_2025-02-20_22-05-31.png" style="zoom:50%;" />

rebase操作就是把从创建分支的版本把某个分支从该版本的后面所有版本移接到另一个分支的最新版本后

```
git rebase 分支名
```

在 Git 中，`rebase` 并不会直接合并代码。它的作用是将一个分支的变更（通常是你的工作分支）应用到另一个分支上（通常是主分支或目标分支）之上。具体来说，`rebase` 会将你的提交逐个“摘下来”，然后将目标分支的提交“合并”到你的分支上，最后将你的提交应用到更新后的分支上。

可以把它理解为一个重新整理提交的过程，它会让你的提交历史看起来像是线性发展的。这种方式不会像 `merge` 那样产生一个新的合并提交，而是让你的提交历史显得更加清晰，避免了多次合并产生的多余“合并提交”。

举个例子，假设你有以下的提交历史：

```
scss复制编辑A---B---C (master)
     \
      D---E (feature)
```

如果你执行 `git rebase master`，`feature` 分支的提交（D 和 E）会被“重新应用”到 `master` 分支的最新提交 C 之后，变成这样：

```
mathematica


复制编辑
A---B---C---D'---E' (feature)
```

注意：这里的 D' 和 E' 是基于 C 的新提交，原来的 D 和 E 会被丢弃（或者说它们变得不可见了）。所以，`rebase` 并不会合并代码，而是将你自己的工作“重新放到”目标分支上，并且可以使代码历史更加整洁。

如果想要合并代码，通常会使用 `merge`，它会将两个分支的变更合并在一起并产生一个新的提交。

## 15.分支管理和工作流模型

git flow模式

Git Flow 是一种常见的 Git 分支管理模型，它帮助团队在进行软件开发时，通过组织分支的策略来保持开发过程的清晰、规范和高效。Git Flow 提出了明确的分支规则，适用于大多数需要稳定版本发布的项目，尤其是在多人协作的大型项目中。

Git Flow 模型的核心思想是通过不同类型的分支来管理项目的不同阶段，通常包括以下几种分支类型：

1. **Master 分支（主分支）**

- **作用**：`master` 分支是用来保存代码的稳定版本的分支，通常对应于可以发布的版本。
- **特点**：每次发布的代码都会合并到 `master` 分支，且 `master` 分支上的每个提交都应该是稳定的、可部署的。

2. **Develop 分支（开发分支）**

- **作用**：`develop` 分支是用来进行日常开发工作的主分支，它包含了当前所有开发中最新的功能。
- **特点**：开发人员从 `develop` 分支派生出新的功能分支（feature branch），完成的功能通常先合并回 `develop` 分支。`develop` 分支是 `master` 的开发版本，未经过最终测试的代码会在这里。
- **合并策略**：所有的功能开发在 `develop` 分支上完成，然后经过测试和确认合并到 `master`。

3. **Feature 分支（功能分支）**

- **作用**：`feature` 分支用于开发新的功能或特性。每个 `feature` 分支对应一个特定的功能，通常从 `develop` 分支创建。
- **命名规则**：`feature/功能名`，例如：`feature/login-page`。
- **生命周期**：开发完成后，`feature` 分支会合并回 `develop` 分支。每个 `feature` 分支通常较短，开发完成后就删除。
- **工作流程**：从 `develop` 创建一个新的 `feature` 分支，开发完后再将其合并回 `develop`。

4. **Release 分支（发布分支）**

- **作用**：`release` 分支用于准备一个新版本的发布。它从 `develop` 分支创建，并用于进行最终的修复、测试和准备工作。
- **命名规则**：`release/版本号`，例如：`release/1.0.0`。
- **生命周期**：一旦发布准备好，`release` 分支会合并回 `master` 分支，并且会标记一个版本号（如 git tag）。同时，它也会合并回 `develop` 分支，以便将发布期间的修复同步到 `develop`。
- **工作流程**：从 `develop` 分支创建 `release` 分支，进行最后的准备和修复，发布后合并回 `master` 和 `develop`。

5. **Hotfix 分支（紧急修复分支）**

- **作用**：`hotfix` 分支用于修复 `master` 分支中的生产环境中的问题。通常是在发布后发现的 bug 或者是紧急的修复。
- **命名规则**：`hotfix/版本号`，例如：`hotfix/1.0.1`。
- **生命周期**：修复完成后，`hotfix` 分支会同时合并回 `master` 和 `develop` 分支。`master` 会同步到一个新的版本，`develop` 会保持更新，以防止丢失修复的内容。
- **工作流程**：从 `master` 创建一个新的 `hotfix` 分支，修复问题后合并回 `master` 和 `develop`。

Git Flow 的工作流程

Git Flow 的工作流程从创建分支到发布版本一般按以下顺序进行：

1. **开始新功能开发**：

   - 从 

     ```
     develop
     ```

      分支创建一个新的 

     ```
     feature
     ```

      分支：

     ```
     bash复制编辑git checkout develop
     git checkout -b feature/feature-name
     ```

   - 在 `feature` 分支上开发新的功能。

   - 功能开发完成后，将 

     ```
     feature
     ```

      分支合并回 

     ```
     develop
     ```

     ：

     ```
     bash复制编辑git checkout develop
     git merge feature/feature-name
     git branch -d feature/feature-name  # 删除已合并的 feature 分支
     ```

2. **准备发布版本**：

   - 当 

     ```
     develop
     ```

      上的功能准备好进行发布时，从 

     ```
     develop
     ```

      创建一个 

     ```
     release
     ```

      分支：

     ```
     bash复制编辑git checkout develop
     git checkout -b release/1.0.0
     ```

   - 在 `release` 分支上进行最后的修复、文档更新等工作。

   - 一旦准备好，

     ```
     release
     ```

      分支合并回 

     ```
     master
     ```

      并打标签：

     ```
     bash复制编辑git checkout master
     git merge release/1.0.0
     git tag 1.0.0
     ```

   - 同时，

     ```
     release
     ```

      分支需要合并回 

     ```
     develop
     ```

     ，以确保 

     ```
     develop
     ```

      上包含发布时的最后修复：

     ```
     bash复制编辑git checkout develop
     git merge release/1.0.0
     git branch -d release/1.0.0
     ```

3. **紧急修复**：

   - 如果生产环境中发现紧急 bug，可以从 

     ```
     master
     ```

      创建一个 

     ```
     hotfix
     ```

      分支：

     ```
     bash复制编辑git checkout master
     git checkout -b hotfix/1.0.1
     ```

   - 修复完成后，

     ```
     hotfix
     ```

      分支合并回 

     ```
     master
     ```

      和 

     ```
     develop
     ```

     ：

     ```
     bash复制编辑git checkout master
     git merge hotfix/1.0.1
     git tag 1.0.1
     git checkout develop
     git merge hotfix/1.0.1
     git branch -d hotfix/1.0.1
     ```

总结

- **`master`**：稳定的发布版本，随时可部署。
- **`develop`**：开发中的版本，包含最新的开发功能。
- **`feature`**：新功能的开发分支。
- **`release`**：发布准备分支，用于最后的测试和修复。
- **`hotfix`**：用于修复生产环境中的紧急问题。

Git Flow 的优点是让开发过程更加规范，明确区分了功能开发、发布准备和紧急修复。缺点是分支管理较为复杂，特别是在需要频繁发布的项目中，可能会增加一定的管理成本。对于一些小型项目或快速迭代的项目，可能更倾向于使用更加简化的 Git Flow 模型。

<img src="images\Snipaste_2025-02-20_22-19-58.png" />



gitHub flow模式

GitHub Flow 是 Git Flow 的一种简化版本，专门设计用于现代、快速迭代的开发流程，尤其适合于持续交付（Continuous Delivery）和持续集成（Continuous Integration）的项目。GitHub Flow 是一种轻量级、更加灵活的分支管理模型，适合频繁发布更新的项目，特别是在 GitHub 这样的平台上，便于团队合作和代码审查。

GitHub Flow 的关键特点

1. **主分支（main/master）**
   - **作用**：`main` 分支（之前 GitHub 默认是 `master`，但现在推荐使用 `main`）始终代表着稳定、可发布的版本。任何时刻 `main` 分支上的代码都是已经经过测试和审查，能够部署到生产环境的版本。
   - **特点**：开发者不会直接在 `main` 分支上进行开发工作，所有开发任务都是在其他分支上进行的，只有经过代码审查和合并的代码才会进入 `main` 分支。
2. **功能分支（feature branch）**
   - **作用**：开发者基于 `main` 分支创建新的功能分支进行工作，功能分支可以是针对特定 bug 修复、新特性的开发或其他任何任务。
   - **命名规则**：通常是 `feature/描述`，例如：`feature/login-page`。
   - **特点**：开发人员在自己的 `feature` 分支上进行开发，完成后通过 Pull Request 提交到 `main` 分支。每次任务完成后都应该删除相应的分支。

GitHub Flow 工作流程

1. **创建功能分支**：

   - 开始开发之前，开发人员会从 

     ```
     main
     ```

      分支创建一个新的功能分支。这个分支上会进行一系列的开发工作，直到功能完成。

     ```
     bash复制编辑git checkout main  # 切换到 main 分支
     git pull origin main  # 拉取最新的代码
     git checkout -b feature/your-feature-name  # 创建新分支
     ```

2. **进行开发并提交代码**：

   - 在功能分支上开发并提交代码。开发完成后，进行本地的测试，确保代码没有问题。

     ```
     bash复制编辑git add .  # 添加修改
     git commit -m "Add feature X"  # 提交更改
     git push origin feature/your-feature-name  # 推送到远程仓库
     ```

3. **发起 Pull Request**：

   - 一旦功能开发完成，就会创建一个 Pull Request (PR)，将自己的功能分支提交给 `main` 分支进行合并。通常会在 PR 中描述自己的修改内容，并请求团队成员进行代码审查。
   - 在 GitHub 上，创建 PR 后，其他团队成员会进行代码审查，提出建议或要求修改。

4. **进行代码审查和合并**：

   - 团队成员会检查 PR，并对代码提出修改建议。如果代码没有问题，就可以将功能分支合并到 `main` 分支。
   - 在 GitHub 上，PR 可以进行线上讨论，经过批准后，通常会使用“Squash and Merge”或“Merge”按钮将代码合并到 `main` 分支。

5. **部署到生产环境**：

   - 一旦代码合并到 `main` 分支，通常意味着代码是稳定的，可以部署到生产环境。许多项目使用 CI/CD 工具（如 GitHub Actions、Jenkins 等）来自动化构建、测试和部署过程。

6. **删除功能分支**：

   - 合并后的功能分支不再需要，开发者通常会删除本地和远程的功能分支，以保持仓库整洁。

     ```
     bash复制编辑git branch -d feature/your-feature-name  # 删除本地分支
     git push origin --delete feature/your-feature-name  # 删除远程分支
     ```

GitHub Flow 的优势

1. **简化的流程**：
   - 与 Git Flow 相比，GitHub Flow 不需要维护大量的发布分支、开发分支和热修复分支，只需要 `main` 和功能分支，流程非常简洁和直观。
2. **持续集成和部署**：
   - GitHub Flow 强调的是持续集成（CI）和持续部署（CD），每个功能分支都是相对独立的，因此可以更快速地部署和发布新功能。
3. **频繁的发布和反馈**：
   - 由于每个功能分支在完成后会迅速发起 PR，团队可以快速获得代码审查和反馈，促进开发效率。同时，随着功能不断合并到 `main`，项目可以更频繁地进行发布，确保产品的快速迭代。
4. **灵活性**：
   - GitHub Flow 非常灵活，适合那些需要快速迭代、频繁发布的项目。团队成员可以根据实际情况选择不同的开发策略，避免了过多的分支管理。
5. **增强团队协作**：
   - Pull Request 的流程促进了团队成员之间的协作和讨论，使得每个变更都可以经过多方审查，减少了错误和质量问题。

总结

GitHub Flow 提供了一种简单、灵活且高效的开发流程，特别适用于需要快速迭代、持续集成和持续部署的项目。与 Git Flow 相比，它的分支模型更简化，专注于 `main` 分支和功能分支，强调代码审查和自动化部署，非常适合现代软件开发和团队协作。



<img src="images\Snipaste_2025-02-20_22-19-17.png" />
