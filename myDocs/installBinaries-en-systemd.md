# Node installation with *systemd*

Edited in 2025 18th may.

Based on the version  MAIN.2.5.1

Thanks to JEROMEH and Oliver Wild Boar on the Massa Discord for reviewing and reporting issues.

## Introduction

I'm using binaries with Linux.


## 0. Update

If you have previously followed this tutorial and want to install a new version, you only need to redo the following steps some new things with 2.5.1 version:

1. Stop *massad.service* daemon:
```sh
sudo systemctl stop massad.service
sudo systemctl disable massad.service
```

2. Go to the personnal folder
```sh
cd ~/
```

3. Download the archive
```sh
https://github.com/massalabs/massa/releases/download/MAIN.2.5.1/massa_MAIN.2.5.1_release_linux.tar.gz
```

4. Uncompress the archive
```sh
tar xzf massa_MAIN.2.5.1_release_linux.tar.gz
```

5. To update 2.4 version and before to 2.5.1 version, you need to modify **/etc/systemd/system/massad.service**, we add `-a` option (to accpet automaticaly the [Massa community chart](https://github.com/massalabs/massa/blob/main/COMMUNITY_CHARTER.md)) in line to start `massa-node` :
```desktop
ExecStart=/home/[USER]/massa/massa-node/massa-node -a -p YourPassword
```

6. we update services:
```sh
sudo systemctl daemon-reload
```

7. WARNING! If you have a `[bootstrap]` section with bootstrap nodes from a previous version, you need to update them.

8. We restart the node
```sh
sudo systemctl enable massad
sudo systemctl restart massad
```

9. We buy rolls like in the [8. Buy rolls](https://github.com/JeromeSi/TraductionsFrMassaDoc/blob/main/myDocs/installBinaries-en-systemd.md#8-buy-rolls) without forgetting `node_start_staking AU...`

## 1. Download binaries

Go to the web page where you can find all releases : [the link](https://github.com/massalabs/massa/releases/) to select the last version.

For Linux, you need to download a file with name like ***massa_MAIN.X.X_release_linux.tar.gz*** where **X.X** is the number of version, actually **2.3**.

Without a graphic interface, we use **wget** for downloading :

- to install it use : **sudo apt install wget**

- go to the user directory with : **cd**

- to download, use : **wget https://github.com/massalabs/massa/releases/download/MAIN.2.5.1/massa_MAIN.2.5.1_release_linux.tar.gz** . Verify the version, here it's **2.5.1**.

## 2. Uncompress the file

If **tar** isn't install, you install it with **sudo apt install tar**

We use **tar** with our file like : **tar xzf massa_MAIN.2.5.1_release_linux.tar.gz** where **2.5.1** is the number of the version.

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

```toml
[protocol]
	routable_ip = "Your IPv4 or IPv6 quoted-quote"
#You need to a new line after the line with IP
```

We edit with **nano** : **nano ~/massa/massa-node/config/config.toml**

With **nano**, we save it with [Ctrl]+[o] and we close with [Ctrl]+[x]

Note : if you use an IPv6, your IP may change continually, it's the [SLAAC]([IPv6 — Wikipédia]([IPv6 address - Wikipedia](https://en.wikipedia.org/wiki/IPv6_address#Stateless_address_autoconfiguration))'s fault. You will have to manually set an IPv6 address to your node.

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

```desktop
[Unit]
	Description=Massa Node
	After=network-online.target
[Service]
	User=root
	WorkingDirectory=/home/[USER]/massa/massa-node
	ExecStart=/home/[USER]/massa/massa-node/massa-node -a -p YourPassword
	Restart=on-failure
	RestartSec=3
	LimitNOFILE=65535
[Install]
	WantedBy=multi-user.target
```

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

We activate this service for an automatic start when OS boot :

`sudo systemctl enable massad.service`

### 3. Verification of the operation of the *massad* service

Check that everything works with :

`sudo systemctl status massad`

We leave this display with [Ctrl]+[c]

### 4. Read the node log file *Massa*

We use :

`sudo journalctl -u massad -f`

We leave this display with [Ctrl]+[c]

### 5. Read the node log file *Massa* between 2 moments

We use :

`sudo journalctl -u massad -f --since="2023-03-10 15:45" --until="2023-03-10 16:45"


### 6. Stop *massad* service

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

## 7. Is the node connected to the Massa network ?

We use the client :

- **cd ~/massa/massa-client/**

- **./massa-client -p thePassword**

The node joins the Massa network by doing a *bootstrap*.

The **get_status** command returns a lot of information when the node is connected to the Massa network. In this case, refer to the paragraph explaining how to buy a roll.

If **get_status** returns a red error message, there is an issue."

We search the informations in the last lines of the log file with **sudo journalctl -u massad -f**

If you read :

- *Bootstrap failed because the bootstrap server currently has no slots available.* : the bootstrap process did not occur because the servers are unavailable; you just need to be patient.

- *Error while connecting to bootstrap server: io error: Connection refused (os error 111)* : you need to check that the ports are open correctly (on the machine and possibly on the box).

- *Error while bootstrapping: `massa_signature` error Signature error* : you can try to stop the node, delete the wallet, create a new one and start the node. Or delete the boostrap list in file **~/massa/massa-node/config/config.toml** if you have add another bootstrap node.

- *Your last bootstrap on this server was ...s ago and you have to wait ...s before retrying* : You are entitled to a bootstrap every 12 hours. You can either wait to connect to a server where the bootstrap has not been attempted, or you can find a list of nodes to bootstrap from, but they may not be official.

- Another option is to ask your question to the community [Massa](https://massa.net/community) Or you could look for additional information [lien EN](https://github.com/JeromeSi/TraductionsFrMassaDoc/blob/main/myDocs/bootstrapErrorsAndExplanations.md)

## 8. Buy rolls

To buy rolls, you need Massa. During the testnet, Massa has no value and is generously distributed.

You need to use the staking address obtained by using **node_get_staking_addresses** in the client, or **wallet_info** (in the latter case, it is named *Address*).

The sources of Massa are :

- on [Discord](https://discord.gg/massa), You need to send your staking address on the channel *#testnet-faucet*

- sur [paranorm.pro](http://mfaucet.paranorm.pro/), You need to send your staking address and prove that you are not a robot

The massa comes in less than 1 minute on the wallet and we can spend them as soon as we see `Balance: final` with more than 100Massa (**wallet_info** in **massa-client**).

We use **buy_rolls StakingAddress 1 0.01** to buy one roll (cost 100Mass) with 0.01 fees.

It take 3 cycles for the roll become active which is a maximum of  1h42min24s.

## 9. Official docs

Go to the doc :

- [in english](https://docs.massa.net/)
