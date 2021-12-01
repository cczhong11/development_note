# install

```
brew install zsh
```

oh-my-zsh

```
wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | sh
```

## themes

clone到  ~/.oh-my-zsh/themes
修改`~/.zshrc` ZSH_THEME=" "

- git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
- powerlevel10k/powerlevel10k

## plugin

- autojump 
  - brew install autojump
- zsh-autosuggestions
  - git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
- zsh-syntax-highlighting
  - git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
- bat
  - add syntax in cat
- sudo
  - type esc esc twice to add sudo
## tip

- 在当前目录下输入 .. 或 ... ，或直接输入当前目录名都可以跳转，你甚至不再需要输入 cd 命令了。
- 目录浏览和跳转：输入 d，即可列出你在这个会话里访问的目录列表，输入列表前的序号，即可直接跳转。
- 命令参数补全。键入 kill <tab> 就会列出所有的进程名和对应的进程号
- 通配符搜索：ls -l **/*.sh，可以递归显示当前目录下的 shell 文件，文件少时可以代替 find。使用 **/ 来递归搜索
- zsh_stats: Get a list of the top 20 commands and how many times they have been run.
- take: Create a new directory and change to it, will create intermediate directories as required.
- x / extract: Extract an archive (supported types: tar.{bz2,gz,xz,lzma}, bz2, rar, gz, tar, tbz2, tgz, zip, Z, 7z).
- osx

## alias 

| Flag | Description                                    |
| ---- | ---------------------------------------------- |
| L    | print each alias in the form of calls to alias |
| g    | list or define global aliases                  |
| m    | print aliases matching specified pattern       |
| r    | list or define regular aliases                 |
| s    | list or define suffix aliases                  |

## Directory
| Flag  | Description                           |
| ----- | ------------------------------------- |
| ..    | cd ..                                 |
| ...   | cd ../..                              |
| ....  | cd ../../..                           |
| ..... | cd ../../../..                        |
| /     | cd /                                  |
| ~     | cd ~                                  |
| md    | mkdir -p                              |
| rd    | rmdir                                 |
| d     | dirs -v (lists last used directories) |

## osx
| Flag        | Description                                         |
| ----------- | --------------------------------------------------- |
| tab         | Open the current directory in a new tab             |
| split_tab   | Split the current terminal tab horizontally         |
| vsplit_tab  | Split the current terminal tab vertically           |
| ofd         | Open the current directory in a Finder window       |
| pfd         | Return the path of the frontmost Finder window      |
| pfs         | Return the current Finder selection                 |
| cdf         | cd to the current Finder directory                  |
| pushdf      | pushd to the current Finder directory               |
| pxd         | Return the current Xcode project directory          |
| cdx         | cd to the current Xcode project directory           |
| quick-look  | Quick-Look a specified file                         |
| man-preview | Open a specified man page in Preview app            |
| showfiles   | Show hidden files in Finder                         |
| hidefiles   | Hide the hidden files in Finder                     |
| itunes      | DEPRECATED. Use music from macOS Catalina on        |
| music       | Control Apple Music. Use music -h for usage details |
| spotify     | Control Spotify and search by artist, album, track… |
| rmdsstore   | Remove .DS_Store files recursively in a directory   |