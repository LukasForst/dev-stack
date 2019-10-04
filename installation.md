## Applications
* keepass - https://keepassxc.org/
* drop down terminal 
  * tilda  https://github.com/lanoxx/tilda
  * guake
  * XFCE drop down terminal `xfce4-terminal --drop-down`
* VLC
* JetBrains Toolbox
* Opera + Firefox + Chromium
* LibreOffice - `libreoffice-still`
* vmtouch - adds files to ram
* Spotify
* Sublime text
* Make - support for the makefiles
* Kotlin Script
* Gradle
* Maven

### OpenVPN
Support for XFCE4 running on Manjaro, but probably will work on other systems as well. Using following tool https://wiki.archlinux.org/index.php/Networkmanager-openvpn (installed by default on XFCE since it is its default Network manager).
```bash
nmcli connection import type openvpn file <my.file>.ovpn
```

It adds the connection to the network manager, then one must enable it by clicking on it.

### Mini Greeter
`lightdm-mini-greeter` - lightweight lightdm lock screen, also it fixed not showing lightdm after sleep.

Greeter requires minimal configuration - at least to set current user, which should be logged in when the password is entered.
The configuration is stored in the `/etc/lightdm/lightdm-mini-greeter.conf`.
```
user = lukas
```

To enable greeter one must set `greeter-session` in the file `/etc/lightdm/lightdm.conf` in a following way:
```
greeter-session=lightdm-mini-greeter
```


### Albert
Configuration
* theme: `Numix Rounded` (complete black after back and forth switch of frontend)
* show via command `albert toggle`
  * it does not support Win Key -> use XFCE keyboard settings

### Docker + Docker Compose
see [docker file](docker.md)

### Python 3
see [python file](python.md)

### VS Codium
Free build of VS Code https://github.com/VSCodium/vscodium
extensions
* Spell checker - https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker
* Latex - https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop
* Idea bindings keys - https://marketplace.visualstudio.com/items?itemName=k--kato.intellij-idea-keybindings

### create_ap
useful tool, which can create an WIFI access point 
```bash
sudo create_ap --ieee80211ac --freq-band 2.4 --isolate-clients -m bridge wlp2s0 enp1s0 SharedHpWifi HelloStranger!
```

### NBFC
fan control for notebooks - https://github.com/hirschmann/nbfc
* requires .NET runtime - mono
* on Manjaro install `nbfc-git` package for the latest versions

#### Configuration
configuration for HP
```bash
nbfc config -s "HP ProBook 440 G3"
```
configuration for Thinkpad
```bash
<waiting>
```

to start the service run 
```bash
nbfc start -e
```

If you encounter any problem (mainly happen on Manjaro)
```bash
sudo mv /opt/nbfc/Plugins/StagWare.Plugins.ECSysLinux.dll /opt/nbfc/Plugins/StagWare.Plugins.ECSysLinux.dll.old
```
Then restart the service
```bash
systemctl restart nbfc
```
And start it again
```bash
nbfc start -e
```


### Oh My Zsh
Better bash - installation with following command
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

#### Plugins
* syntax highlight must be downloaded
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```
* autosuggestions
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

##### Final plugins
```bash
plugins=(
  git
  bundler
  docker
  docker-compose
  python
  archlinux
  web-search
  battery
  gitignore
  github
  git-flow
  gradle
  npm
  zsh-syntax-highlighting
  zsh-autosuggestions
)
```

#### Aliases
Current state on 2/10/2019
```bash
alias code=codium

alias subl=subl3
alias work='cd ~/work'
alias tasp='cd ~/work/tasp'
alias paas='cd ~/work/tasp/paas'
alias capa='cd ~/work/capa'
alias ktoolz='cd ~/work/ktoolz'

alias school='cd ~/OneDrive/School/ing/1st'
alias onedrive='cd ~/OneDrive'
alias docs='cd ~/OneDrive/Documents'
alias pics='cd ~/OneDrive/Pictures'

alias open='xdg-open'

alias jdownloader='~/.local/share/jdownloader.sh'
alias onedrive='~/.local/share/onedrive.sh'
alias onedrive-logs='docker logs --follow --since 5m onedrive'
alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
alias battery=acpi
alias pingg='ping google.com'
alias tmp='cd /tmp'
alias dops='docker ps'
alias cache-stats='vmtouch "`pwd`"'
alias cache='sudo vmtouch -dl "`pwd`"'
alias aliases='subl ~/.zshrc'
alias size='du -sh'
alias notes='code ~/OneDrive/Documents/notes'
alias py='python3'
alias save-aliases='cp -r ~/.zshrc ~/OneDrive/useful'
```