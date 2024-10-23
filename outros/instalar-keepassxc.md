# 🔐 Instalar keepassxc

## Baixar a imagem&#x20;

{% embed url="https://keepassxc.org/download/#linux" %}

## Configurações

Criar pasta para armazenar o keepassxc

```sh
sudo mkdir /opt/keepassxc
```

Entrar dentro da pasta downloads, ou na pasta que você preferiu salvar a imagem e mover para a pasta criada

```sh
mv NOME_DO_APPIMAGE /opt/keepassxc
```

Mover ícone svg desejado para representar o keepassxc nos seus aplicativos

Exemplo:

```sh
mv keepassxc.svg /opt/keepassxc
```

Criar configuração par abrir o arquivo

```sh
sudo vim /usr/share/applications/keepassxc.desktop
```

Salve esse conteúdo dentro do arquivo. OBS: Lembre de conferir se os nome estão corretos

```sh
Version=2.7.9
Name=KeePassXC
Comment=Password manager
Exec=/opt/keepassxc/KeePassXC-2.7.9-x86_64.AppImage
Icon=/opt/keepassxc/keepassxc.svg
Terminal=false
Type=Application
Categories=Utility;Security;Application;

```

Instalar o fuse

```sh
sudo apt install fuse 
```

## Conclusão

Após realizar todos os passos basta pesquisar o aplicativo na sua lista de aplicativos





