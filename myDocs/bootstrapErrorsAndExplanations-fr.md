# Liste des erreurs lors du bootstrap
## Qu'estce que le bootstrap ?
Le bootstrap est une étape. Quand vous faites un bootstrap, votre node se synchronise avec les informations de la blockchain Massa.

## Ou puis-je faire un bootstrap ?
Au départ, vous pouvez faire le bootstrap à partir de la liste des nodes officiels.

Vous trouverez la liste des nodes officiels dans le fichier **massa/massa-node/base_config/config.toml** à la section **[bootstrap]**. 

## Liste de quelques erreurs lors du bootstrap (et ce que l'on peut faire)
1. **Bootstrap from server IPtargetNode:31245 failed. Your node will try to bootstrap from another server in 60s.** : votre node doit attendre **60s** avant de tenter un nouveau bootstrap sur un autre node.
2. **Error while connecting to bootstrap server: io error: No route to host (os error 113)** : le node cible est à l'arrêt ou injoignable. Attendez que votre node essaie avec un autre node présent dans la liste de bootstrap.
3. **Error received from bootstrap server: Your last bootstrap on this server was 11h 59m 43s 895ms 540us 156ns ago and you have to wait before retrying.** : avec les nodes officiels, vous avez droit à une tentative de boostrap toutes les 12h. Dans cet exemple, vous devez attendre 11h 59m. Attendez que votre node fasse un autre essai dans 60s avec un autre node de la liste de bootstrap.
4. **Error received from bootstrap server: Bootstrap failed because the bootstrap server currently has no slots available.** : vous êtes malchanceux, le node cible ne peut pas accepter plus de bootstrap en même temps, soyez patient et attendez, votre node fasse un autre essai dans 60s avec un autre node de la liste de bootstrap.
5. **Error while bootstrapping: io error: early eof** : 2 possibilités
	1. the IP of your node isn't allow to bootstrap with the target node. The target must had your IP in the file **massa/massa-node/base_config/bootstrap_whitelist.json** and restart the target node. It is planned to add dynamically IP in the target node in version 18.
	2. the port 31244 and/or 31245 are not open. Open it in your node and in your box or in the VPS management interface
6. **Error while bootstrapping: `massa_signature` error Signature error : Signature verification failed: signature error: Verification equation was not satisfied** : Verify the signature of the target node in the file where is the list of the bootstrap node. If you add nodes for bootstrap, there is an error when you copy address and/or node ID. You can delete the node or comment the line with **#** at the beginning.
 
## Notes
1. If you have an IPv4, you can't bootstrap on target node with IPv6, only IPv4
2. If you have an IPv6, you can bootstrap on target node with IPv6 or IPv4
