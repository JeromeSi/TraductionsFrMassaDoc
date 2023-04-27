# Parlons bootstrap
## 1. Qu'est-ce que le bootstrap ?
Le bootstrap est une étape. Quand vous faites un bootstrap, votre node se synchronise avec les informations de la blockchain Massa.

## 2. Ou puis-je faire un bootstrap ?
Au départ, vous pouvez faire le bootstrap à partir de la liste des nodes officiels.

Vous trouverez la liste des nodes officiels dans le fichier **massa/massa-node/base_config/config.toml** à la section **[bootstrap]**.

## 3. Autoriser un bootstrap sur son node

Il faut :

1. s'assurer que le port 31245 est ouvert
2. s'assurer que le node est fonctionnel (le client répond à `get_status`)
3. ajouter l'IP du node que l'on veut autoriser dans la liste blanche avec le client en utilisant la commande `node_bootstrap_whitelist add IPv4orIPv6`
4. vérifier que l'IP a été correctement ajouter avec `node_bootstrap_whitelist` qui donne la liste des IP autorisés

*Note* : depuis la version 18 (ou 19), il n'est plus utile de redémarrer le node pour que la nouvelle IP soit prise en compte.

## 4. Ajouter un node dans sa liste de bootstrap

**ATTENTION** : faire un bootstrap sur un node qui n'est pas officel présente un risque pour la sécurité de votre node et des informations qu'il contient (wallet). Durant le testnet, le risque est limité car les Massa n'ont pas encore de valeur mais lors du mainnet, méfiance.

La liste des nodes officielles de bootstrap se trouve dans le fichier `massa/massa-node/base_config/config.toml` à la section `[bootstrap]`.

On crée une section identique dans le fichier de configuration qui nous est personnel `massa/massa-node/config/config.toml` en copiant les informations des nodes officiels :

``````toml
[network]
    routable_ip = "VotreIPv4ouIPv6"

[bootstrap]
    # list of bootstrap (ip, node id)
    bootstrap_list = [
        ["IPnodeOfficiel_1:31245", "N...nodeID_du_node_officiel_1"],
        ["IPnodeOfficiel_2:31245", "N...nodeID_du_node_officiel_2"],
        ["IPnodeOfficiel_3:31245", "N...nodeID_du_node_officiel_3"],
        ["IPnodeOfficiel_4:31245", "N...nodeID_du_node_officiel_4"],
    ]
 ```

Une fois fait, on ajoute les informations de la node non officielle en conservant le formalisme :

- l'IPv4 s'écrit normalement mais l'IPv6 doit être entourée de crochets (`[]`)
- `31245` c'est le numéro du port qui sert au bootstrap
- `"N...nodeID_du_node_officiel_1"` on trouve le node ID en utilisant `get_status` dans le client à la première ligne

Si le node est train de tenter des boostrap durant la modification de `massa/massa-node/config/config.toml`, on l'arrête et on le redémarre.

Si on souhaite ne tenter un redémarrage que sur un node particulier, il suffit d'ajouter un `#` au début de la ligne ou se trouve le node à igno
*Note 1* : on peut limiter les tentatives en ajoutant à un type d'IP en ajoutant `bootstrap_protocol = "IPv4"` dans la section `[bootstrap]` de `massa/massa-node/config/config.toml`

*Note 2* : on peut diminuer le temps entre 2 tentatives en ajoutant à un type d'IP en ajoutant `retry_delay = 15000` dans la section `[bootstrap]` de `massa/massa-node/config/config.toml`


## 5. Liste de quelques erreurs lors du bootstrap (et ce que l'on peut faire)
1. **Bootstrap from server IPtargetNode:31245 failed. Your node will try to bootstrap from another server in 60s.** : votre node doit attendre **60s** avant de tenter un nouveau bootstrap sur un autre node.
2. **Error while connecting to bootstrap server: io error: No route to host (os error 113)** : le node cible est à l'arrêt ou injoignable. Attendez que votre node essaie avec un autre node présent dans la liste de bootstrap.
3. **Error received from bootstrap server: Your last bootstrap on this server was 11h 59m 43s 895ms 540us 156ns ago and you have to wait before retrying.** : avec les nodes officiels, vous avez droit à une tentative de boostrap toutes les 12h. Dans cet exemple, vous devez attendre 11h 59m. Attendez que votre node fasse un autre essai dans 60s avec un autre node de la liste de bootstrap.
4. **Error received from bootstrap server: Bootstrap failed because the bootstrap server currently has no slots available.** : vous êtes malchanceux, le node cible ne peut pas accepter plus de bootstrap en même temps, soyez patient et attendez, votre node fasse un autre essai dans 60s avec un autre node de la liste de bootstrap.
5. **Error while bootstrapping: io error: early eof** : 2 possibilités
	1. l'IP de votre node n'est pas autorisé à réaliser un bootstrap sur ce node. Le node cible doit avoir votre IP dans le fichier `massa/massa-node/base_config/bootstrap_whitelist.json`. Voir le paragraphe **3. Autoriser un bootstrap sur son node**
	2. Les ports 31244 et/ou 31245 sont fermés. Ouvrez les sur votre node et sur votre box internet ou avec l'interface de gestion de votre VPS.
6. **Error while bootstrapping: `massa_signature` error Signature error : Signature verification failed: signature error: Verification equation was not satisfied** : Vérifier le node ID du node cible dans le fichier ou il y a la liste des nodes de bootstrap. Si vous avez ajouté des nodes pour le bootstrap, c'est une erreur qui a pu apparaître quand vous avez fait un copier/coller du node ID. Vous pouvez effacer la ligne du node problématique ou commenter la ligne avec **#** au début.
7. **Error while connecting to bootstrap server: io error: Connection refused (os error 111)** : on peut avoir cette erreur quand **massa-node** a été lancé plusieurs fois ou lors de la première tentative de bootstrap. On ferme toutes les processus massa-node avec `pkill -f massa-node` et il suffit de relancer comme vous savez le faire.
8. **WARN massa_bootstrap::client: Error while connecting to bootstrap server: io error: Network is unreachable (os error 101)** : vous avez sûrement une IPv4 et votre node tente un bootstrap sur un node officiel avec une IPv6. Une IPv4 ne peut contacter que les IPv4 (Voir **Notes**). On peut ajouter ces lignes dans le fichier `massa/massa-node/config/config.toml` :

```toml
[bootstrap]
    # force the bootstrap protocol to use: "IPv4", "IPv6", or "Both". Defaults to using both protocols.
    bootstrap_protocol = "IPv4"
 ```

9. **WARN massa_bootstrap::client: Error while bootstrapping: general bootstrap error: Parsing Error: Failed BootstrapServerMessage deserialization / Failed MIP store deserialization / Failed MipStoreRaw der / Failed mip store stats der / Failed MipStoreStats network version counters der / Failed counters len der / Fail / Input: [15, 0, 15]** : Pour le moment, je ne sais pas d'ou viens le problème mais il devrait être résolu dans la version 22.

10. **Error while connecting to bootstrap server: io error: Cannot assign requested address (os error 99)** : Pour le moment, je ne sais pas d'ou viens le problème.

## Notes
1. Si vous êtes en IPv4, vous ne pouvez pas faire un bootstrap sur un node cible en IPv6, seulement avec une IPv4
2. Si vous êtes en IPv6, vous pouvez faire un bootstrap sur des node en IPv6 ou IPv4
