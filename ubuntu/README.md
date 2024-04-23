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

* ### git

```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt install git -y
```

* ### vs code

```bash
sudo apt-get install wget gpg
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -D -o root -g root -m 644 packages.microsoft.gpg /etc/apt/keyrings/packages.microsoft.gpg
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/keyrings/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
rm -f packages.microsoft.gpg
sudo apt install apt-transport-https
sudo apt update
sudo apt install code # or code-insiders
```

* ### ssh key

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```
* ### chrome
https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
