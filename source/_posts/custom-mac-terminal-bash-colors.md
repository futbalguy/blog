title: 'Terminal Bash Command Line & Colors'
id: 1166
categories:
  - Software Engineering
tags:
  - Hack Reactor
  - Web Development
date: 2015-07-14 20:53:20
thumbnail: /ScreenShotAfter.png
banner: /ScreenShotAfter.png
---

Software developers spend a significant amount of time in Terminal (i.e. file systeming, giting, bash scripting).

The default mostly-white wall of text can be hard on the eyes and actually kind of difficult to read. Wouldn't it be nice if you could more clearly view the terminal logs?

YES, so let's make some changes that will increase producitivity and asthetics at the same time!

<!-- more -->

## Customizing Your Terminal Bash Profile

First, you need to learn a little about bash. bash is a Unix shell and command language and it is what you see when you load Terminal on your Mac. We will be customizing your bash Profile.

The first step is to go to start Terminal, navigate to your home directory, and open .bash_profile in a text editor.

##### terminal
```shell
//navigate to home directory and
//open .bash_profile in a text editor (i.e. sublime)
cd ~
sublime .bash_profile
```

Second, you can customize the bash prompt to show full filepath, hide username, and show your current git branch name (if applicable). We also add a new line to seperate the prompt from the prior command.

##### .bash_profile

```bash
#Git branch in prompt.
parse_git_branch() {
    git branch 2&gt; /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

#custom bash prompt
export PS1=&quot;\n\w\ $(parse_git_branch) &quot;
```

Next, you can customize colors to better differentiate different bash info. ANSI colors are assigned to variables at the top of the bash script.


##### .bash_profile

```bash
# High Intensty Colors
GREENH='\[&#92;&#48;33[0;92m\]' # Green
BLUEH='\[&#92;&#48;33[0;94m\]' # Blue

NC='\[&#92;&#48;33[0m\]' # No color

#Git branch in prompt.
parse_git_branch() {
    git branch 2&gt; /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

#custom bash prompt with colors
export PS1=&quot;\n${BLUEH}\w${GREENH}\$(parse_git_branch)${NC} &quot;

#CLICOLOR will turn colors on or off.
export CLICOLOR=1

#LS_COLORS is not required, and will let you customize the colors
export LSCOLORS=ExFxBxDxCxegedabagacad
```

## Behold The Difference

#### Before

![](/ScreenShotBefore.png)

#### AFTER!!!!

![](/ScreenShotAfter.png)