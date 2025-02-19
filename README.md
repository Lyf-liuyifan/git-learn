# git学习

## 1.初始化配置

这里需要注意的是代码托管网站对本地设定的账户是如何处理的。代码托管网站，主要看email，用email地址来匹配自己的账户名的邮件地址，如果相同，代码托管网站就认为此操作是账户所有者的操作。比如：

如果本地设定的[user.email](http://user.email/)值是：[personal@126.com](mailto:personal@126.com)，由于在GitHub上的账户的邮件地址也是[personal@126.com](mailto:personal@126.com)，如果从这台电脑push的话，GitHub会认定这次这个push是账户拥有者自己做的，跟直接登录到GitHub，从网站上修改，是相同的，修改人是一样，就是账户拥有者。

如果本地设定的[user.email](http://user.email/)值是：[company@company.cn](mailto:company@company.cn)，也能push到GitHub，GitHub会记录这次的修改是另一个人（用户名是company）做的。

另外，还需要注意，user.name和user.email除了设定全局值（–gloabl）之外，还可以在具体的代码目录中设定自己的user.name和user.email值。

[user.name](http://user.name/)和[user.email](http://user.email/)的设定策略就是：设定global值的时候，考虑一下，这台电脑做公司的事多还是自己的事多，比如公司的电脑，明显做公司的事情多，在我们这里最好设定成：[user.email](http://user.email/) = [company@company.cn](mailto:company@company.cn)，[user.name](http://user.name/) = company。当我们需要在公司电脑上干点私活的时候，具体的项目目录中，`git config user.name "personal"`,`git config user.email "personal@126.com"`，把值改成符合自己要求的。



配置本机器的上传时用户名

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

## 2.新建仓库

windows下需要打开gitbash,然后再git bash里进行一系列操作

在所需要创造仓库的文件夹下面直接使用

```
git init
```

就会出现.git的隐藏文件夹

但是此时的本地仓库是没有和远程的仓库进行关联的所以我们还要在gitee上创造一个仓库进行关联，但是不要勾选初始化仓库



这个时候还需要在windows上设置凭证不然需要一直输入用户名以及密码

我的私有令牌apt : b23818132579a449e60710334966829b

---

在 Git 和 Gitee（或 GitHub、GitLab）中，**凭据（用户名 + 访问令牌/密码）** 和 **Git 配置的用户名 & 邮箱** 是两回事，分别用于不同的目的。

------

**1️⃣ 为什么 Git 需要凭据？**

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

这样，你就可以更顺畅地使用 Git 了！ 🚀



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
sh复制编辑touch README.md
echo "# 我的项目" >> README.md
git add README.md
git commit -m "添加 README 文件"
git push origin main
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
sh复制编辑# 进入你的本地仓库目录
cd /path/to/your-repo

# 关联远程仓库（替换为你的 Gitee 仓库地址）
git remote add origin https://gitee.com/your-username/repo-name.git

# 确保关联成功
git remote -v
```

🔹 **使用 HTTPS 时**，Gitee 可能要求你输入 **用户名和个人访问令牌（PAT）** 进行身份验证。

![image-20250206022829305](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20250206022829305.png)

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

## ==3.添加和提交文件==

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

![image-20250206221650917](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20250206221650917.png)

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

![image-20250210235954650](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20250210235954650.png)

写入文件的格式还可以加入通配符

```
*.log
```

==有个值得注意的地方：我们只能忽略不在版本库里的文件，如果已经在版本库里得删除该文件才可以生效==

如果要忽略整个目得在文件名的后面加上/

7.2匹配原则

从上到下每行

![image-20250211001712704](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20250211001712704.png)

![image-20250211001737915](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20250211001737915.png)

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

![image-20250217220202369](C:\Users\liu\AppData\Roaming\Typora\typora-user-images\image-20250217220202369.png)