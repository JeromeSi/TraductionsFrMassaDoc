# Installation d'un node avec *systemd*

Basée sur l'épisode 19.2

## Introduction

J'utilise la version binaire sur une machine Linux.

Mise en garde : le fonctionnement sur un serveur mutualisé (VPS) est aléatoire.

## 1. Récupération de l'archive

On se rend sur la page des différentes versions: [Releases](https://github.com/massalabs/massa/releases) pour sélectionner la dernière disponible.

Pour Linux, le fichier à télécharger sera nommé comme ***massa_TEST.*** **XX.X** ***_release_linux.tar.gz*** ou **XX.X** est le numéro de la version.

Sans interface graphique, on utilise **wget** pour télécharger :

- pour l'installer : **sudo apt install wget**

- on va dans le dossier de l'utilisateur : **cd**

- pour l'utiliser : **wget https://github.com/massalabs/massa/releases/download/TEST.19.2/massa_TEST.19.2_release_linux.tar.gz** Il faudra remplacer les **XX.X** par la version recherchée

## 2. Décompression de l'archive

Si **tar** n'est pas présent, on l'installe avec **sudo apt install tar**

On utilise **tar** sur notre archive : **tar xzf massa_TEST.19.2_release_linux.tar.gz** ou **XX.X** est le numéro de la version.

Avec **ls**, vous pouvez voir que vous avez un dossier **massa** dans lequel se trouve tout le nécessaire.

## 3. Installation d'une bibliothèque

Vous devez installer la bibliothèque **libssl1** avec **sudo apt install libssl1**

## 4. Mise en place de l'accessibilité du node

On commence par vérifier si on a ou pas un pare-feu actif : **sudo ufw status**

Il y a 2 possibilités :

- pas de pare-feu comme sur ce [lien](https://i0.wp.com/thelinuxcode.com/wp-content/uploads/2018/02/Terminal-augusto@thelinuxcode-_013.png?resize=579%2C160&ssl=1).

- pare-feu actif comme sur ce [lien](https://www.lifewire.com/thmb/uTsuw9NaujyxRdkwsB4fZeP8oTw=/993x803/filters:no_upscale():max_bytes(150000):strip_icc()/ufwdisable_2-5c6c406446e0fb0001917358.jpg).

Si vous voulez activer votre pare-feu, il faut commencer par autoriser les liaisons SSH surtout si vous l'utilisez pour contacter votre machine avec **sudo ufw allow 22** puis on active le pare-feu avec **sudo ufw enable**

Il faut ouvrir 3 ports (33244, 33245 et 33035) comme suit:

- **sudo ufw allow 31244** (permet d'être contacté par d'autres nodes présents dans la blockchain)

- **sudo ufw allow 31245** (permet le bootstrap sur le node et d'être considéré comme routable par MassaBot)

- **sudo ufw allow 333035** (permet d'obtenir des informations sur le node depuis un site externe [lien en](https://docs.massa.net/en/latest/testnet/community-resources.html#tracking-the-activity-of-a-node))

Il faut aussi connaître son IP publique pour la noter dans le fichier **~/massa/massa-node/config/config.toml** que l'on obtient avec **host myip.opendns.com resolver1.opendns.com** Elle est sur la dernière ligne.

Le fichier **~/massa/massa-node/config/config.toml** doit contenir :

`[network]`
`routable_ip = "Votre IPv4 ou IPv6 entourée de guillemet"`

On fait l'édition avec **nano** : **nano ~/massa/massa-node/config/config.toml**

Avec **nano**, on enregistre avec [Ctrl]+[o] et on ferme avec [Ctrl]+[x]

## 5. Mise en marche du node avec *systemd*

### 1. Création du fichier pour le service *massad*

Il faut créer le fichier **/etc/systemd/system/massad.service** avec **sudo nano /etc/systemd/system/massad.service**.
Dans ce fichier, on écrit (ou on copie-colle) :

`[Unit]
Description=Massa
NodeAfter=network-online.target
[Service]
User=rootWorkingDirectory=/home/[USER]/massa/massa-node
ExecStart=/home/[USER]/massa/massa-node/massa-node -p [PASSWORD]
Restart=on-failure
RestartSec=3
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target`

Avec **nano**, on enregistre avec [Ctrl]+[o] et on ferme avec [Ctrl]+[x]

On rend le fichier **/etc/systemd/system/massad.service** exécutable par tous le monde avec :

`sudo chmod 777 /etc/systemd/system/massad.service`

### 2. Lancement du service *massad*

On démarre le service *Massa* avec :

`sudo systemctl start massad`

### 3. Vérification du fonctionnement du service *massad*

On vérifie que tout fonctionne avec :

`sudo systemctl status massad`

On ressort de cet affichage avec [Ctrl]+[c]

### 4. Lecture du fichier de log du node *Massa*

On utilise :

`sudo journalctl -u massad -f`

On ressort de cet affichage avec [Ctrl]+[c]

### 5. Arrêt du service *massad*

On arrête le service *Massa* avec :

`sudo systemctl stop massad`

Attention ! Si vous arrêtez le node avec `node_stop`dans le client, le *systemd* va le redémarrer automatiquement.

### 6. Mais j'ai déjà vu ça quelque part...

J'ai repris le travail qui se trouve ici : [Manage your Massa node with SystemD]([Manage your Massa node with SystemD - MassAdopted.com](https://massadopted.com/manage-your-massa-node-with-systemd/))

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

- *Your last bootstrap on this server was ...s ago and you have to wait ...s before retrying* : on a droit à un bootstrap toutes les 12h, il faut attendre de tomber sur un serveur ou le bootstrap n'a pas été tenté ou se trouver une liste de node pour le faire mais ils ne seront pas officiels

- autre chose : allez poser votre question à la communauté [Massa](https://massa.net/community) ou d'autres informations [lien FR](https://github.com/JeromeSi/TraductionsFrMassaDoc/blob/main/myDocs/bootstrapErrorsAndExplanations-fr.md)

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
  
  