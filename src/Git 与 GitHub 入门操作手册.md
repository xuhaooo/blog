> 不要一开始就试图理解 git 的原理，我目前也不理解，当下只需要背下这些命令，按步骤配置好 git 和 GitHub 使用它们即可，理解原理这种事还是等熟练使用之后再谈吧。
## 一、 配置 GitHub
1. 首先你得用一个 GitHub 账号，没有就去注册
2. 进入[SSH and GPG keys](https://github.com/settings/keys)
3. 点击 New SSH key
4. title 随便填，key 等一会填
5. 打开 Git Bash，键入 `rm -rf ~/.ssh/*`（不要打错哪怕一个空格，否则后果自负），把现有的 SSH key 都删掉
6. 键入`ssh-keygen -t rsa -b 4096 -C "你的邮箱"`，真的要填入你的邮箱哦！
7. 按回车三次，别问中间发生了什么
8. 键入`cat ~/.ssh/id_rsa.pub`，得到一串东西，完整复制这段东西，回到页面上，粘贴这个 ‘key’
9. 点击 add SSHkey
10. 回到 Git Bash，键入 `ssh -T git@github.com`，你可能会看到一个提示（提示最后问你 yes/no），输入 yes 并回车，别傻等着
11. 然后，如果你看到 Permission denied(public)，就说明你失败了，那就回到第 2 步重来
12. 如果你看到跟 Hi XXX! You've successfully authenticated,but Github does not provide shell access.差不多的提示，就说明你成功了

- 好了，终于添加好了一个 SSH key 了，不要问这个有什么用，想要了解原理就去看这篇[文章](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)，不过我建议你先不要看，还是那句话，先用起来再说
- 一台电脑只需要一个 SSH key
- 一个 SSH key 可以访问你 GitHub 的所有仓库，
- 如果你买了新电脑，新电脑上新生成一个 SSH key ，将这个 key 也上传到 GitHub 上，它与之前的 key 可以共存
- 如果你不小心将 key 从电脑上删除了，重新生成一个即可，替换之前的

## 二、配置 git
```
git config --global user.name 你的英文名
git config --global user.email 你的邮箱
git config --global push.default matching
git config --global core.quotepath false
git config --global core.editor "vim"
```
- 打开 Git Bash ，依次运行上面 5 句命令，每句回车后都没有反应，没有反应就对了，弄完你的 git 就配置好了

## 三、使用 git 的三种方式
### 1. 只在本地使用
- 打开 Git Bash 进入一个你常用的目录，创建我们的项目目录 `mkdir git-demo-1`
- 进入目录 `cd git-demo-1`
- `git init`，这句命令会在 git-demo-1 中创建一个 .git 目录，`ls -al` 可以看到它，不过不用进去看
- 在 git-demo-1 中添加任意文件，假如我们添加了两个文件，分别是 index.html 和 css/style.css
```
touch index.html
mkdir css
touch css/style.css
```
- 运行 `git status -sb`，可以看到文件前面有 ?? 号
```
## Initial commit on master
?? /css
?? index.html
这个 ?? 号表示 git 对你的操作很懵逼，不知道你要怎么对待这些变动
```
- 使用 `git add` 将文件添加的“暂存区”
- 你可以一个一个地 add
```
git add index.html
git add css/style.css
```
- 你也可以一次性地 add
```
git add .
表示将当前目录下的变动都添加到“暂存区”
```
- 再次运行 `git status -sb`，可以看到 ?? 变成了 A
```
## Initial commit on master
A css/style.css
A index.html
这个 A 表示添加，你告诉了 git ，这些文件我要添加到仓库里
```
- 使用 `git commit -m "信息"` 将你 add 的内容“正式提交”到本地仓库（.git），并添加了注释信息，方便日后查阅
- 你可以一个一个地 commit
```
git commit index.html -m ‘添加 index.html’
git commit css/style.css -m ‘添加 css/style.css’
```
- 你也可以一次性地 commit
```
git commit . -m ‘添加了几个文件’
```
- 再次运行 `git status -s` 发现没有文件变动了，这是因为文件变动已经记录在仓库了
- 你可以用 `git log` 你的 git 历史变动
```
commit f0d8gfgfg98hjg9f8g0s9f8g9s8gs98fg9800fgs
Author: XXX<XXX@xxx.com>
Date: Thu Sep 27 22:30:33 2018 +0800
添加几个文件
```
- 以上就是 git add/git commit 的一次完整过程，可得出来还是挺复杂的，原则上来说，错了一步就重新来过，直到不再手抖为止
#### 如果我想继续更改文件，应该怎么办?
1. git-demo-1 目录下 `start css/style.css` 会用默认编辑器打开 css/style.css（start/open）
2. 编辑输入 `body {background: red}` 保存退出
3. `git status -sb` 发现提示中有个红色的 M
```
M css/style.css
M （即 Modified），表示这个文件被修改了
```
4. 此时你如果想让改动保存到仓库中，你需要 `git add css/style.css`或 `git add .`
5. 运行 `git status -sb` 发现 M 由红色变成了绿色，先别管是啥意思，记住：先 add 再 commit
6. `git commit -m ‘更新 css/style.css’` 将这个改动添加到 .git 仓库
7. `git status -sb` 发现没有变更了，这说明所有变动都被本地仓库记录在案了
- 注：git status -sb 用来显示当前文件状态的，看哪个文件变动了，方便进行 git add 操作
- -sb: -s 表示总结（summary）-b 表示分支（branch）

#### 本地使用总结：
- `git init`，初始化本地仓库 .git
- `git status -sb`，显示当前所有文件状态
- `git add 文件路径`，用来将变动加到暂存区
- `git commit -m ‘信息’`，用来正式提交变动，提交至 .git 仓库
- 如果没有新的变动，只需依次执行 `git add xxx` 和 `git commit -m ‘xxx’` 两个命令即可
- `git log` 查看变更历史


### 2. 将本地仓库上传到 GitHub
> 如何将我们创建的 git-demo-1 上传到 GitHub 呢？
1. 点击 GItHub 上的 New repository 新建一个仓库，仓库名字（Repository name）可以随意，一般跟本地目录名一致，也叫 git-demo-1
2. 其他什么都别选,记住了，然后点击 Create repodsitory 创建仓库，GitHub 会把后续的操作全都告诉你
3. 点击第一行中的 SSH 按钮（划重点），如果你不点击就会默认使用 HTTPS 地址，千万不要用 HTTPS 地址，使用起来特别麻烦，每次都要输入用户名密码
4. 下面 3 个方框里面的命令里面也会同步你选用的地址，所以命令中应该有 `git@github.com:xxx/git-demo-1.git`这样的内容，而不是 `https://github.com/xxx/git-demo-1.git` 这样的
5. 下面 3 个框中的命令就是 GitHub 告诉你后续的操作，框 1 是在 GitHub 上新建远程仓库时用的，框 2 就是本地上传仓库到 GitHub 上要用的
6. 分别复制这两行并运行，刷新页面本地仓库就被上传到 GitHub 上了，搞定

### 3. 下载 GitHub 上的仓库到本地
> 除了前面两种用法，还有一种就是直接在 GitHub 上创建一个仓库，然后下载到本地
1. 跟第一种方法一样，创建一个仓库，不过这次设置完仓库名 git-demo-2 后，需要更改一些设置，Description（描述）随便写，选择 Public，勾选 Initialize this repository with a README，Add .gitignore 选择 Node，Add a License 选择 MIT License，然后创建仓库
2. 这样仓库中就自动包含了 3 个文件 .gitignore、README.md、LISENCE，想要了解它们的作用请自行谷歌([.gitignore](http://gitbook.liuhui998.com/4_1.html)、[LICENSE](http://www.ruanyifeng.com/blog/2011/05/how_to_choose_free_software_licenses.html))
3. 远程仓库已经创建好了，想要下载到本地需要使用 `git clone` 命令，点击绿色按钮 clone and download 会看到一个弹出层，确保里面的地址是 SSH 地址（即点击弹出层右上角 Use SSH），然后复制框中的 SSH 地址
4. 打开 Git Bash，进入一个安全的目录，比如桌面 `cd ~/Desktop`
5. 运行 `git clone 复制的 SSH 地址`，你会发现桌面上多了一个 git-demo-2 的目录
6. 进入这个目录 `cd git-demo-2`
7. `ll -la` 你会发现远程目录里的文件都在这出现了，你还会看到 .git 本地仓库，这时你就可以添加文件了，就是上面说过的 add 然后 commit

### 4. 如何上传更新
> 如果本地目录有任何变动，只需按照以下顺序就能上传：
1. git add 文件路径
2. git commit -m '信息'
3. git pull （当你的远程仓库被人更改过了，你直接 git push 不会成功，得先 pull 到本地再 push）
4. git push
```
cd git-demo-1
touch index2.html
git add index2.html
git commit -m '新建 index2.html'
git pull 
git push
```
- 这样你在 git-demo-1 的页面就能看到 index2.html 了，easy

## 四、.gitignore
- 在项目创建 .gitignore 文件就可以指定那些文件不上传到远程仓库，比如：
```
/node_modules/
/.cache/
/.DS_Store/
```
- 这样就可以避免这些目录被上传到 Github 上了
- 记住：永远不要上传 node_modules 到 GitHub

## 五、 其他
- 还有一些有用的命令：
```
git remote add origin git@github.com:xxx.git 将本地仓库与远程仓库关联
git remote set-url origin git@github.com:xxx.git 上一步手抖可以用此命令挽回
git branch 新建分支
git merge 合并分支
git diff 查看详细变化
git revert/git reset 两种后悔
```

## 六、 资源
- [常见 Git 命令清单](http://www.ruanyifeng.com/blog/2015/12/git-cheat-sheet.html)
- [菜鸟 Git 教程](https://www.runoob.com/git/git-tutorial.html)
- [读懂 diff](http://www.ruanyifeng.com/blog/2012/08/how_to_read_diff.html)
- [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html)
- [廖雪峰 Git 教程](https://www.liaoxuefeng.com/wiki/896043488029600)

