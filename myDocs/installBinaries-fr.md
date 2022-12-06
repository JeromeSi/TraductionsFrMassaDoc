# Installation d'un node avec *nohup*

Basée sur l'épisode 17

## Introduction

J'utilise la version binaire sur une machine Linux.

Mise en garde : le fonctionnement sur un serveur mutualisé est aléatoire.

## 1. Récupération de l'archive

On se rend sur la page des différentes versions: [Releases](https://github.com/massalabs/massa/releases) pour sélectionner la dernière disponible.

Pour Linux, le fichier à télécharger sera nommé comme ***massa_TEST.*** **XX.X** ***_release_linux.tar.gz*** ou **XX.X** est le numéro de la version.

Sans interface graphique, on utilise **wget** pour télécharger :

- pour l'installer : **sudo apt install wget**

- on va dans le dossier de l'utilisateur : **cd**

- pour l'utiliser : **wget https://github.com/massalabs/massa/releases/download/TEST.XX.X/massa_TEST.XX.X_release_linux.tar.gz** Il faudra remplacer les **XX.X** par la version recherchée

## 2. Décompression de l'archive

Si **tar** n'est pas présent, on l'installe avec **sudo apt install tar**

On utilise **tar** sur notre archive : **tar xzf massa_TEST.XX.X_release_linux.tar.gz** ou **XX.X** est le numéro de la version.

Avec **ls**, vous pouvez voir que vous avez un dossier **massa** dans lequel se trouve tout le nécessaire.

## 3. Installation d'une bibliothèque

Vous devez installer la bibliothèque **libssl1** avec **sudo apt install libssl1**

## 4. Mise en place de l'accessibilité du node

On commence par vérifier si on a ou pas un pare-feu actif : **sudo ufw status**

Il y a 2 possibilités :

- pas de pare-feu comme sur ce [lien](https://i0.wp.com/thelinuxcode.com/wp-content/uploads/2018/02/Terminal-augusto@thelinuxcode-_013.png?resize=579%2C160&ssl=1).

- pare-feu actif comme sur ce [lien](https://www.lifewire.com/thmb/uTsuw9NaujyxRdkwsB4fZeP8oTw=/993x803/filters:no_upscale():max_bytes(150000):strip_icc()/ufwdisable_2-5c6c406446e0fb0001917358.jpg).

Si vous voulez activer votre pare-feu, il faut commencer par autoriser les liaisons SSH surtout si vous l'utilisez pour contacter votre machine avec :**sudo ufw allow 22** puis on active le pare-feu avec **sudo ufw enable**

Il faut ouvrir 3 ports (33244, 33245 et 33035) comme suit:

- **sudo ufw allow 31244**

- **sudo ufw allow 31245**

- **sudo ufw allow 333035**

Il faut aussi connaître son IP publique pour la noter dans le fichier **~/massa/massa-node/config/config.toml** que l'on obtient avec **host myip.opendns.com resolver1.opendns.com** Elle est sur la dernière ligne.

Le fichier **~/massa/massa-node/config/config.toml** doit contenir :

`[network]`
`routable_ip = "Votre IPv4 ou IPv6 entourée de guillemet"`

On fait l'édition avec **nano** : **nano ~/massa/massa-node/config/config.toml**

## 5. Mise en marche du node avec *nohup*

On se place dans le dossier **~/massa/massa-node/** avec **cd ~/massa/massa-node/**

On exécute **massa-node** en le détachant du terminal avec **nohup ./massa-node -p leMotDePasse &>> ./logs.txt &** Il ne doit s'afficher que le numéro associé au processus si tout est ok.

## 6. Création du wallet

On se place dans le dossier **~/massa/massa-client/** avec **cd ~/massa/massa-client/**

On exécute **massa-client** avec **./massa-client -p leMotDePasse**

Vous êtes désormais dans le client qui vient d'afficher l'aide. On génère le wallet avec **wallet_generate_secret_key**

On en profite pour déclarer notre wallet prêt à staker des blocs avec **node_add_staking_secret_keys VotreCléSecreteIci**

On peut vérifier que l'opération s'est bien passée avec **node_get_staking_addresses**

On sort du client avec **exit**

## 7. Est-ce que le node est connecté au réseau Massa ?

On va utiliser le client :

- **cd ~/massa/massa-client/**

- **./massa-client -p leMotDePasse**

L'opération pour rejoindre le réseau Massa s'appelle le *bootstrap*.

La commande **get_status** renvoie plein d'informations quand le node est connecté au réseau Massa. Dans ce cas, rendez-vous au paragraphe expliquant l'achat d'un roll.

Si **get_status** renvoie un message d'erreur en rouge, il y a un problème.

On va chercher les informations dans les dernières lignes du fichier de log avec **tail ~/massa/massa-node/logs.txt**

Vous lisez :

- *Bootstrap failed because the bootstrap server currently has no slots available.* : le bootstrap ne s'est pas fait car il n'y a pas de disponibilité des serveurs, il faut juste être patient

- *Error while connecting to bootstrap server: io error: Connection refused (os error 111)* : il faut vérifier que les ports sont bien ouverts (machine + box éventuellement)

- *Error while bootstrapping: `massa_signature` error Signature error* : mauvais signe car je n'ai jamais vu ça avec la version binaire, à part la réinstallation rien à proposer

- *Your last bootstrap on this server was ...s ago and you have to wait ...s before retrying* : on a droit à un bootstrap toutes les 12h (ou 8h), il faut attendre de tomber sur un serveur ou le bootstrap n'a pas été tenté ou se trouver une liste de node pour le faire mais ils ne seront pas officiels

- autre chose : allez poser votre question à la communauté [Massa](https://massa.net/community)

## 8. Acheter des rolls

Pour acheter des rolls, il faut des Massa. Durant le testnet, les Massa n'ont aucune valeur et sont distribués avec générosité.

Il faut utiliser l'adresse de staking que l'on obtient en utilisant **node_get_staking_addresses** dans le client ou **wallet_info** (dans ce dernier cas, c'est celle nommée *Address* )

Les sources sont :

- sur [Discord](https://discord.gg/massa), il faut envoyer son adresse de staking sur le channel *#testnet-faucet*

- sur [paranorm.pro](http://mfaucet.paranorm.pro/), il faut envoyer son adresse de staking et prouvez que l'on n'est pas un robot

Les massa arrivent en moins d'une minute sur le wallet et on peut les dépenser dès que l'on voit le `Balance: final` avec plus de 100Massa (**wallet_info** dans **massa-client**).

On utilise **buy_rolls adresseDeStaking 1 0** pour acheter 1 roll (qui coûte 100Mass) avec 0 fees.

Il faut 3 cycles pour que le roll deviennent actif soit 1h42min24s au maximum.

## 9. Le système de récompenses

Voir la doc :

- [en anglais](https://docs.massa.net/en/latest/testnet/rewards.html)

- [en français](https://github.com/JeromeSi/TraductionsFrMassaDoc/blob/main/githubMassaLabs/rewards.md)
