# Node installation with *systemd* in debug mode

Based on the episode 22.1

## Introduction

I'm using binaries with Linux.

We are going to set the node to debug level 3. This will generate larger log files. To avoid running out of disk space, we will set up a quota for the Massa service log.

It takes about around **70MB per hour** for logging (1.64GB per day).

## 0. Mise à jour

If you have previously followed this tutorial and want to install a new version, you only need to redo the following steps:

1. Save `config.toml` and your wallet :
```sh
cp ~/massa/massa-node/config/config.toml ~/
cp ~/massa/massa-client/wallet.dat ~/
```
2. Delete the `masssa` folder
```sh
rm -r ~/massa
```
3. Download archive
4. Uncompress archive
5. Copy `config.toml` and your wallet:
```sh
cp ~/config.toml ~/massa/massa-node/config/
cp ~/wallet.dat ~/massa/massa-client/
```
6. Restart as in 5.3. Run *massad* service

## 1. Download binaries

Go to the web page where you can find all releases : [the link](https://github.com/massalabs/massa/releases) to select the last version.

For Linux, you need to download a file with name like ***massa_TEST.*** **XX.X** ***_release_linux.tar.gz*** where **XX.X** is the number of version.

Without a graphic interface, we use **wget** for downloading :

- to install it use : **sudo apt install wget**

- go to the user directory with : **cd**

- to download, use : **wget https://github.com/massalabs/massa/releases/download/TEST.22.1/massa_TEST.22.1_release_linux.tar.gz** . Verify the version, here it's **22.1**.

## 2. Uncompress the file

If **tar** isn't install, you install it with **sudo apt install tar**

We use **tar** with our file like : **tar xzf massa_TEST.22.1_release_linux.tar.gz** where **22.1** is the number of the version.

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
[logging]
    # Logging level. High log levels might impact performance. 0: ERROR, 1: WARN, 2: INFO, 3: DEBUG, 4: TRACE
    level = 3

[network]
	routable_ip = "Your IPv4 or IPv6 quoted-quote"
#You need to a new line after the line with IP
```

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

```desktop
[Unit]
	Description=Massa Node
	After=network-online.target
[Service]
	User=root
	WorkingDirectory=/home/[USER]/massa/massa-node
	ExecStart=/home/[USER]/massa/massa-node/massa-node -p YourPassword
	Restart=on-failure
	RestartSec=3
	LimitNOFILE=65535
	LogNamespace=massa
[Install]
	WantedBy=multi-user.target
```

Adapting to your situation:

+ Replace `[USER]' with the user ID of the user who runs Massa.

+ Replace `YourPassword` with the password you will use to execute `massa-node`

+ If you use `root`tu run Massa (not recommended) , you must change `/home/[USER]`par `/root`

+ You can choose the directory where the log files will be saved by adding `LogsDirectory=/mnt/logsMassa` in the `[Service]` section of the `/etc/systemd/system/massad.service` file.

With **nano**, we save it with [Ctrl]+[o] and we close with [Ctrl]+[x].

We make executable the file **/etc/systemd/system/massad.service** for everyone with :

`sudo chmod 777 /etc/systemd/system/massad.service`

### 2. Creating the file to limit the size of the log for the *massad* service.

The file **/etc/systemd/journal@massa.conf** must be created with **sudo nano /etc/systemd/journal@massa.conf**.
In this file, we write (or make a copy-paste) :

```desktop
[Journal]
    SystemMaxUse=21G
```

Adapting to your situation:

+ It's up to you to choose the size of the log based on the available space.


### 2. Run *massad* service

We use the *Massad* service with :

`sudo systemctl start massad`

We activate this service for an automatic start when OS boot :

`sudo systemctl enable massad.service`

### 4. Verification of the operation of the *massad* service

Check that everything works with :

`sudo systemctl status massad`

We leave this display with [Ctrl]+[c]

### 5. Read the node log file *Massa*

We use :

`sudo journalctl --namespace massa -f`

We leave this display with [Ctrl]+[c]

### 6. Read the node log file *Massa* between 2 moments

We use :

`sudo journalctl --namespace massa -f --since="2023-03-10 15:45" --until="2023-03-10 16:45"


### 7. Stop *massad* service

We stop the *massad* service with :

`sudo systemctl stop massad`

Warning ! If you stop the node with `node_stop`in the client, the *systemd* will restart it automatically.

### 8. But I’ve seen this before...

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

We use **buy_rolls StakingAddress 1 0** to buy one roll (cost 100Mass) with 0 fees.

It take 3 cycles for the roll become active which is a maximum of  1h42min24s.

## 9. The reward system

Go to the doc :

- [en anglais](https://docs.massa.net/en/latest/testnet/rewards.html)
