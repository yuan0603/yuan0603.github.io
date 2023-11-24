## Ubuntu

* ### .bash_aliases
```bash
alias ll="ls -lAvF --time-style='+%Y-%m-%d %H:%M:%S' --group-directories-first --color=always"
alias la="ls -lavF --time-style='+%Y-%m-%d %H:%M:%S' --group-directories-first --color=always"
alias lh="ls -lAvFh --time-style='+%Y-%m-%d %H:%M:%S' --group-directories-first --color=always"

alias grep="grep --color=always"
```

* ### .bash_profile
```bash
if [ -f ~/.bashrc ]; then 
    source ~/.bashrc 
fi

# set PATH so it includes user's private bin if it exists
if [ -d "$HOME/.local/bin" ] ; then
    PATH="$PATH:$HOME/.local/bin"
fi
```
