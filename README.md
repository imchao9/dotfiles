# dotfiles
my mac dotfiles

https://www.jianshu.com/p/2822a1899f95

Github 上的 Dotfiles 管理
在使用 Linux 和 Mac 系统时候， 常常需要同步 dotfiles。比如 fish 的配置文件在 ~/.config/fish/config.fish，vim 的配置文件在 ~/.vim/vimrc 等等。Github 有个 非官方指南 。

使用 Git Repo 方式管理 dotfiles
参考: The best way to store your dotfiles: A bare Git repository

开始
git init --bare $HOME/.cfg
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
config config --local status.showUntrackedFiles no
echo "alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'" >> $HOME/.bashrc
第一行创建一个文件夹 ~/.cfg，一个 Git bare repository ，用来管理我们的 dotfiles。

创建一个 alias 名为 config 的命令，用来进行操作。

隐藏其他我们没有明确是否跟踪的文件。这样，当使用 config status 命令的时候，不会显示其他无关的文件。

将这个别名命令写入 .bashrc 文件。

执行完以上设置之后，$HOME 文件夹中的任何文件都可以用命令进行版本控制。例如：

config status
config add ~/.vim/vimrc
config commit -m 'Add vimrc'
config add ~/.bashrc
config commit -m 'Add bashrc'
这时候，就可以把这个 git 仓库 push 到 github 或者其他远程仓库上。

在新系统上下载和使用设置
首先依旧新建 alias，忽略 .cfg 文件夹。
alias config='/usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME'
echo ".cfg" >> .gitignore
下载备份的配置
git clone --bare <git-repo-url> $HOME/.cfg
检出分支内容
config checkout
上面的步骤可能会失败，并显示类似的如下消息：

error: The following untracked working tree files would be overwritten by checkout:
    .bashrc
    .gitignore
Please move or remove them before you can switch branches.
Aborting
将这些已经存在的文件移动到备份文件夹：
mkdir -p .config-backup && \
config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | \
xargs -I{} mv {} .config-backup/{}
再次检出

config checkout
这样就可以一直更新并 push 自己的配置仓库了。

作者：北冢
链接：https://www.jianshu.com/p/2822a1899f95
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
