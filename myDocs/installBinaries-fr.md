# Installation d'un node

Basée sur l'épisode 16

## Introduction

J'utilise la version binaire sur une machine Linux.

## 1. Récupération de l'archive

On se rend sur la page des différentes versions: [Releases](https://github.com/massalabs/massa/releases) pour sélectionner la dernière disponible.

Pour Linux, le fichier à télécharger sera nommé comme ***massa_TEST.*** **XX.X** ***_release_linux.tar.gz*** ou **XX.X** est le numéro de la version.

Sans interface graphique, on utilise *wget* :

- pour l'installer : **sudo apt install wget**

- on va dans le dossier de l'utilisateur : **cd**

- pour l'utiliser : **wget https://github.com/massalabs/massa/releases/download/TEST.XX.X/massa_TEST.XX.X_release_linux.tar.gz** Il faudra remplacer les **XX.X** par la version recherchée

## 2. Décompression de l'archive

Si **tar** n'est pas présent, on l'installe avec **sudo apt install tar**

On utilise **tar** sur notre archive : **tar xzf massa_TEST.XX.X_release_linux.tar.gz** ou **XX.X** est le numéro de la version.

Avec **ls**, vous pouvez voir que vous avez un dossier **massa** dans lequel se trouve tout le nécessaire.

## 3. Mise en place de l'accessibilité du node

On commence par vérifier si on a ou pas un pare-feu actif : **sudo ufw status**. Il y a 2 possibilités :

- pas de pare-feu comme sur ce [lien](https://amazingrdp.com/wp-content/uploads/2021/05/Screenshot_1.pnghttps://amazingrdp.com/wp-content/uploads/2021/05/Screenshot_1.png).

- pare-feu actif comme sur ce [lien](https://www.lifewire.com/thmb/uTsuw9NaujyxRdkwsB4fZeP8oTw=/993x803/filters:no_upscale():max_bytes(150000):strip_icc()/ufwdisable_2-5c6c406446e0fb0001917358.jpg).

Si vous voulez activer votre pare-feu, il faut commencer par autoriser les liaisons SSH surtout si vous l'utilisez pour contacter votre machine avec :**sudo ufw allow 22** puis on active le pare-feu avec **sudo ufw enable**

Il faut ouvrir 3 ports (33244, 33245 et 33035) comme suit:

- **sudo ufw allow 31244**

- **sudo ufw allow 31245**

- **sudo ufw allow 333035**

- 

Il faut aussi connaître son IP publique pour la noter dans le fichier **~/massa/massa-node/config/config.toml** que l'on obtient avec **host myip.opendns.com resolver1.opendns.com**. Elle est sur la dernière ligne.

Le fichier **~/massa/massa-node/config/config.toml** doit contenir :

```[network]

```




