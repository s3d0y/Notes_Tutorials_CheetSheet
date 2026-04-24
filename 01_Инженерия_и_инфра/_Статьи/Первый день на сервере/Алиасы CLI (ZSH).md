
Почитай статью [[создание alias]]

Мои Алиас на на локальной машине выгялядят так.  Для ZSH они находятся в файле .zshrc


``` sh
=============================================
 Custom Aliases
=============================================
# Server Management
alias wake-server="echo 'wake' | nc -u 192.168.88.1 9999 -w 1 && echo '✅ Сервер включается...'"
alias ssh-server="ssh home-server"
alias ssh-admin="ssh home-server-admin"

# Navigation
alias ..="cd .."
alias ...="cd ../.."
alias ....="cd ../../.."

# Utilities
alias ll="ls -alF"
alias la="ls -A"
alias l="ls -CF"
alias myip="curl ifconfig.me"
alias ports="netstat -tulpn"

# System info
alias meminfo="free -h"
alias diskusage="df -h"
alias cpuinfo="lscpu"

# Add ~/bin to PATH
export PATH="$HOME/bin:$PATH"
```