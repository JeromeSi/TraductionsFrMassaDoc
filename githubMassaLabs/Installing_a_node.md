# Installation d’un nœud

Traduction de “[Installing a node](https://docs.massa.net/en/latest/testnet/install.html)”
Conforme à la version 22.0 de Massa

> Note

> Actuellement 4 cœurs et 8Gb de RAM doivent suffire pour faire fonctionner un nœud, mais ces caractéristiques devraient augmenter dans le temps. Plus d’info dans la [FAQ](https://docs.massa.net/en/latest/testnet/faq.html#testnet-faq).

## Avec Thyra (le plus simple)

Si voulez lancer un node Massa en seulement quelques clics, vous pouvez télécharger l'application Thyra
et le plugin de gestion de node. Cela vous permettra de configurer, installer et démarrer et même de mettre à jour  un node avec une interface graphique simple. Vous avez 5 étapes à suivre :

+ Installer la version de Thyra pour votre système d'exploitation (OS):
    + [Windows](https://github.com/massalabs/thyra/releases/latest/download/thyra-installer_windows_amd64.exe)
    + MacOS
        + Pour les Apple avec processeur Silicon (M1, M2…), télécharger [ARM](https://github.com/massalabs/thyra/releases/latest/download/thyra-installer_darwin_arm64.tar.gz)
        + Pour les processeurs Intel (i5, i7…), télécharger [AMD](https://github.com/massalabs/thyra/releases/latest/download/thyra-installer_darwin_amd64.tar.gz)
    + [Linux](https://github.com/massalabs/thyra/releases/latest/download/thyra-installer_linux_amd64.tar.gz)
+ Lancer Thyra en utilisant l'icône. Si vous avez besoin d'aide, cette [page](https://github.com/massalabs/thyra/blob/main/INSTALLATION.md) peut être utile.
+ Configurer votre VPS et revenir à Thyra.
+ Installer le plugin de gestion de node.
+ Ajouter la configuration de votre VPS et cliquer sur “start”.

Vous êtes un "node-runner" !

> Note

> Si vous avez des problèmes en suivant cette procédure, n'hésiter pas à [poser une question](https://github.com/massalabs/thyra/issues/new) concernant votre problème.

## Depuis les binaires

Si vous souhaitez juste faire fonctionner un nœud Massa sans avoir à le compiler, vous pouvez simplement télécharger la dernière version ci dessous et passez à l’étape suivante : Démarrer un nœud.

Rendez vous sur la [page en anglais](https://docs.massa.net/en/latest/testnet/install.html#from-binaries) pour un lien de téléchargement à jour.

+ [Windows executable](https://github.com/massalabs/massa/releases/download/TEST.22.0/massa_TEST.22.0_release_windows.zip) fonctionne pour toutes les versions de Windows
+ [Linux binary](https://github.com/massalabs/massa/releases/download/TEST.22.0/massa_TEST.22.0_release_linux.tar.gz) fonctionne au moins pour les versions Ubuntu et Debian avec une la bibliobthèque libc2.28 minimum
+ [MacOS binary](https://github.com/massalabs/massa/releases/download/TEST.22.0/massa_TEST.22.0_release_macos.tar.gz) fonctionne pour les machine Apple

## Depuis le code source

Si vous souhaitez utiliser le code source pour compiler, il faut suivre les étapes suivantes :

### Sur Ubuntu / Debian / MacOS

+ Avec Ubuntu / Debian, ces bibliothèques doivent être installées : `sudo apt install pkg-config curl git build-essential libssl-dev libclang-dev cmake` 
+ Avec MacOS: `brew install llvm`
+ Installation de rustup: `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
+ Configuration du chemin : `source $HOME/.cargo/env`
+ Vérification de la version de rust : `rustc --version`
+ Installation de la version quotidienne (nigthly) : `rustup toolchain install nightly-2023-02-27`
+ En faire la version par défaut : `rustup default nightly-2023-02-27`
+ Vérification de la version de rust : `rustc --version`
+ Cloner le dépôt Massa : `git clone --branch testnet https://github.com/massalabs/massa.git`

### Sur Windows

#### Mise en place de l’environnement de compilation Rust

+ Avec Windows, en premier, vous devez suivre les instructions de Microsoft pour être capable de faire fonctionner un environnement Rust, [c’est ici](https://docs.microsoft.com/en-gb/windows/dev-environment/rust/setup).
  + Installation de “Visual Studio” (recommandé) ou le “ Microsoft C++ Build Tools”.
  + Une fois que “Visual Studio” est installé, click sur “C++ Build Tool”. Selectionner sur la colonne de droite nommée “détails d’installation (“installation details”) les paquets suivants:
    + MSCV v142 — VS 2019
    + Windows 10 SDK
    + C++ CMake tools for Windows
    + Testing Tools Core Feature
  + Click installer sur le bouton droit pour télécharger et installer ces paquets
+ Install NASM : [https://www.nasm.us/pub/nasm/releasebuilds/2.16.01/](https://www.nasm.us/pub/nasm/releasebuilds/2.16.01/) choose win32 or win64 folder depending on your architecture and download then launch the installer.
+ Installation de [Chocolatey](https://docs.chocolatey.org/en-us/choco/setup) et exécution : `choco install llvm`
  + Installation de Rust, [téléchargeable ici](https://www.rust-lang.org/tools/install)
  + Installation de Git pour Windows, [téléchargeable ici](https://git-scm.com/download/win)

#### Cloner le dépôt de Massa

+ Ouvrir “Windows Power Shell”
  + Cloner la dernière version : `git clone --branch testnet https://github.com/massalabs/massa.git`
  + Changer la version par défaut de Rust pour la version quotidienne : `rustup default nightly-2023-02-27`

Suivant : [Démarrer un nœud](./Running_a_node.md) / [Running a node](https://docs.massa.net/en/latest/testnet/running.html)
