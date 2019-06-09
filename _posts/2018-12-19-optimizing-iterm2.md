---
layout: post
title: 优化Mac上的iTerm2
date: 2018-12-19 12:05:08
tags:  奇技淫巧
---
> MacOS自带的Terminal在功能上不够强大,一般都会用iTerm2来替代。但是iTerm2还是有许多可以优化的地方！！


### zsh
##### 主要功能有
* 命令高亮 （识别 命令 正确性） 拓展性高
* 支持 命令补全

#### 安装
```
#安装xcode Command Line Tools 如果已安装则逃过这步
$ xcode-select --install  

$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

$ brew install zsh git autojump

$ chsh -s /bin/zsh

$ sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting

#代码高亮插件
$ echo "source \$ZSH_CUSTOM/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc

#代码自动补全插件
$ git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions
```
#### 配置zsh
```
$ vim ~/.zshrc
```
添加以下内容到.zshrc
```
ZSH_THEME="avit"  #主题 可选其他的
plugins=(
    git
    zsh-autosuggestion  # autosuggestion
)
export HOMEBREW_NO_AUTO_UPDATE=true #关闭brew的自动更新
source $ZSH_CUSTOM/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh
source $ZSH_CUSTOM/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
```
然后使修改立即生效
```
$ source ~/.zshrc
```

### fzf
##### 主要功能有  
* 查找文件  
* 历史命令查询 
* 快速进入目录

##### 安装
```
$ brew install fd fzf
$ /usr/local/opt/fzf/install
$ source ~/.zshrc
```
##### 配置
1）fzf添加到环境中
```
$ vim ~/.zshrc
```
添加以下内容到.zshrc中
```
export FZF_DEFAULT_COMMAND='fd --type file'
export FZF_CTRL_T_COMMAND=$FZF_DEFAULT_COMMAND
export FZF_ALT_C_COMMAND="fd -t d . "
```
然后使修改立即生效
```
$ source ~/.zshrc
```

2）快速显示历史使用命令
```
$ vim /usr/local/opt/fzf/shell/key-bindings.zsh
```
修改以下内容
```
- 66 bindkey '\ec' fzf-cd-widget
+ 66 bindkey '^\' fzf-cd-widget
```
然后使修改立即生效
```
$ source /usr/local/opt/fzf/shell/key-bindings.zsh
```
选择就可以通过在iterm2按`^+R`实现查看之前使用过到命令了

3）文件预览功能
```
$ vim ~/.zshrc
```
添加以下代码到.zshrc文件
```
alias pp='fzf --preview '"'"'[[ $(file --mime {}) =~ binary ]] && echo {} is a binary file || (highlight -O ansi -l {} || coderay {} || rougify {} || cat {}) 2> /dev/null | head -500'"'"
alias oo='fzf --preview '"'"'[[ $(file --mime {}) =~ binary ]] && echo {} is a binary file || (highlight -O ansi -l {} || coderay {} || rougify {} || tac {}) 2> /dev/null | head -500'"'"  # flashback
```
然后使修改立即生效
```
$ source ~/.zshrc
```
设置完别名之后 利用在命令行中输入`pp`  即可完成文件的预览,  `oo`用于倒叙预览文件 在一些流数据文件中比较方便