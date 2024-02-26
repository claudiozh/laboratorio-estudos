---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 📠 O que instalo em uma máquina ubuntu

Instalar

* Dbeaver [https://dbeaver.io/download](https://dbeaver.io/download/)​
* Chrome
* Firefox
* Docker and docker-compose
* VsCode [https://code.visualstudio.com/docs/?dv=linux64\_deb](https://code.visualstudio.com/docs/?dv=linux64\_deb)​
* git
* asdf [https://asdf-vm.com/guide/getting-started.html](https://asdf-vm.com/guide/getting-started.html)​
* nodejs with asdf [https://github.com/asdf-vm/asdf-nodejs](https://github.com/asdf-vm/asdf-nodejs)​
* golang with asdf [https://github.com/asdf-community/asdf-golang](https://github.com/asdf-community/asdf-golang)​
* bun with asdf [https://github.com/cometkim/asdf-bun](https://github.com/cometkim/asdf-bun)​
* Terminator
* KeypassXC
* Discord [https://discord.com/download](https://discord.com/download)​
* Insomnia [https://insomnia.rest/download](https://insomnia.rest/download)​​
* Zsh
* ohmyz [https://ohmyz.sh/#install](https://ohmyz.sh/#install)​
* rclone [https://rclone.org/install/](https://rclone.org/install/)​

sudo su# install docker and docker-composewget -qO- https://get.docker.com/ | shservice docker startsystemctl enable dockercurl -L https://github.com/docker/compose/releases/download/v2.24.2/docker-compose-\`uname -s\`-\`uname -m\` -o /usr/local/bin/docker-composechmod +x /usr/local/bin/docker-compose​# install gitapt install git​# install terminatorapt install terminator​# install keypassxc+snap install keepassxc​# install zshapt install zsh​

#### Install asdf  <a href="#install-asdf" id="install-asdf"></a>

\# Siga a documentação para checar a ultima versão disponível e fazer a configuração correta - https://asdf-vm.com/guide/getting-started.htmlgit clone https://github.com/asdf-vm/asdf.git \~/.asdf --branch v0.14.0

#### asdf install nodejs e golang <a href="#asdf-install-nodejs-e-golang" id="asdf-install-nodejs-e-golang"></a>

asdf plugin add nodejs https://github.com/asdf-vm/asdf-nodejs.gitasdf plugin add golang https://github.com/asdf-community/asdf-golang.gitasdf plugin add bun​asdf install nodejs 20.11.0asdf install golang 1.21.6asdf install bun 1.0.25asdf install php 7.4.3​asdf global nodejs 20.11.0asdf global golang 1.21.6asdf global bun 1.0.25
