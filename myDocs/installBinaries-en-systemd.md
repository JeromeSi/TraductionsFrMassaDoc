# Node installation with *systemd*

Based on the episode 20.1

## Introduction

I'm using binaries with Linux.

## 1. Download binaries

Go to the web page where you can find all releases : [the link](https://github.com/massalabs/massa/releases) to select the last version.

For Linux, you need to download a file with name like ***massa_TEST.*** **XX.X** ***_release_linux.tar.gz*** where **XX.X** is the number of version.

Without a graphic interface, we use **wget** for downloading :

- to install it use : **sudo apt install wget**

- go to the user directory with : **cd**

- to download, use : **wget https://github.com/massalabs/massa/releases/download/TEST.20.1/massa_TEST.20.1_release_linux.tar.gz** . Verify the version, here it's **20.1**.

## 2. Uncompress the file

If **tar** isn't install, you install it with **sudo apt install tar**

We use **tar** with our file like : **tar xzf massa_TEST.20.1_release_linux.tar.gz** where **20.1** is the number of the version.

With **ls**, you can you have a new folder named **massa** where you have all you need.

## 3. Installation of a library

You need to install the library **libssl1** with **sudo apt install libssl1**

## 4. Routability of your node

### 1. In the hardware of your node

You test if you have a firewall : **sudo ufw status**

You've got 2 possibilities :

- no firewall like [lien](https://i0.wp.com/thelinuxcode.com/wp-content/uploads/2018/02/Terminal-augusto@thelinuxcode-_013.png?resize=579%2C160&ssl=1).

- active firewall like [lien](https://www.lifewire.com/thmb/uTsuw9NaujyxRdkwsB4fZeP8oTw=/993x803/filters:no_upscale():max_bytes(150000):strip_icc()/ufwdisable_2-5c6c406446e0fb0001917358.jpg).

If you want to activate your firewall, you must **first** allow the SSH connections especially if you use et to contact your node with **sudo ufw allow 22** and we activate the firewall with **sudo ufw enable**

Open 3 ports (33244, 33245 and 33035) as follows:

- **sudo ufw allow 31244** (allows to be contacted by other nodes present in the blockchain)

- **sudo ufw allow 31245** (allows bootstrap on the node and to be considered routable by MassaBot)

- **sudo ufw allow 33035** (allows to obtain information about the node from an external site [lien en](https://docs.massa.net/en/latest/testnet/community-resources.html#tracking-the-activity-of-a-node))

You must also know your public IP to write it in the file **~/massa/massa-node/config/config.toml** that we get with **host myip.opendns.com resolver1.opendns.com** She’s on the last line.

The file **~/massa/massa-node/config/config.toml** must contain :

`[network]`
`routable_ip = "Your IPv4 or IPv6 quoted-quote"`

We edit with **nano** : **nano ~/massa/massa-node/config/config.toml**

With **nano**, we save it with [Ctrl]+[o] and we close with [Ctrl]+[x]

Note : if you sue an IPv6, your IP may change continually, it's the [SLAAC]([IPv6 — Wikipédia]([IPv6 address - Wikipedia](https://en.wikipedia.org/wiki/IPv6_address#Stateless_address_autoconfiguration))'s fault. You will have to manually set an IPv6 address to your node.

### 2. On the equipment of your ISP

How to do this will depend on each operator.

If you are on IPv4, you must do port forwarding on the box.

If you are on IPv6, you must open a port on the box firewall for the IPv6 address of the node.

## 5. Set up of your node with *systemd*

### 0. Introduction

A service is software that must always work.

A service is managed by the OS and restarts as soon as it stops working.

The service that will be responsible for monitoring *massa-node* is named **massad**.

### 1. Create the file for the service *massad*

The file **/etc/systemd/system/massad.service** must be created with **sudo nano /etc/systemd/system/massad.service**.
In this file, we write (or make a copy-paste) :

`[Unit]`

`Description=Massa Node`

`After=network-online.target`

`[Service]`

`User=root`

`WorkingDirectory=/home/[USER]/massa/massa-node`

`ExecStart=/home/[USER]/massa/massa-node/massa-node -p YourPassword`

`Restart=on-failure`

`RestartSec=3`

`LimitNOFILE=65535`

`[Install]`

`WantedBy=multi-user.target`

Adapting to your situation:

Replace `[USER]' with the user ID of the user who runs Massa.

+ Replace `YourPassword` with the password you will use to execute `massa-node`

+ If you use `root`tu run Massa (not recommended) , you must change `/home/[USER]`par `/root`

With **nano**, we save it with [Ctrl]+[o] and we close with [Ctrl]+[x].

We make executable the file **/etc/systemd/system/massad.service** for everyone with :

`sudo chmod 777 /etc/systemd/system/massad.service`

### 2. Run *massad* service

We use the *Massad* service with :

`sudo systemctl start massad`

### 3. Verification of the operation of the *massad* service

Check that everything works with :

`sudo systemctl status massad`

We leave this display with [Ctrl]+[c]

### 4. Read the node log file *Massa*

We use :

`sudo journalctl -u massad -f`

We leave this display with [Ctrl]+[c]

### 5. Stop *massad* service

We stop the *massad* service with :

`sudo systemctl stop massad`

Warning ! If you stop the node with `node_stop`in the client, the *systemd* will restart it automatically.

### 6. But I’ve seen this before...

I went back to work here : [Manage your Massa node with SystemD]([Manage your Massa node with SystemD - MassAdopted.com](https://massadopted.com/manage-your-massa-node-with-systemd/))

## 6. Creating du wallet

We go in the folder **~/massa/massa-client/** with **cd ~/massa/massa-client/**

We run **massa-client** with **./massa-client -p YourPassword**

You are now in the customer who just displayed the help. We generate the wallet with **wallet_generate_secret_key**

We take this opportunity to declare our wallet ready to stake blocks with **node_start_staking_secret_keys YourAddressHere**

We can verify that the operation went well with **node_get_staking_addresses**

We get out of the client with **exit**

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
