# Liste des erreurs lors du bootstrap
## Qu'est-ce que le bootstrap ?
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
	1. l'IP de votre node n'est pas autorisé à réaliser un bootstrap sur ce node. Le node cible doit avoir votre IP dans le fichier **massa/massa-node/base_config/bootstrap_whitelist.json** et avoir redémarrer. Il est prévu que l'on puisse autoriser, à la volée, des IP dans la version 18.
	2. Les ports 31244 et/ou 31245 sont fermés. Ouvrez les sur votre node et sur votre box internet ou avec l'interface de gestion de votre VPS.
6. **Error while bootstrapping: `massa_signature` error Signature error : Signature verification failed: signature error: Verification equation was not satisfied** : Vérifier le node ID du node cible dans le fichier ou il y a la liste des nodes de bootstrap. Si vous avez ajouté des nodes pour le bootstrap, c'est une erreur qui a pu apparaître quand vous avez fait un copier/coller du node ID. Vous pouvez effacer la ligne du node problématique ou commenter la ligne avec **#** au début.
7. **Error while connecting to bootstrap server: io error: Connection refused (os error 111)** : on peut avoir cette erreur quand :
    1. **massa-node** a été lancé plusieurs fois ou lors de la première tentative de bootstrap. On ferme toutes les processus massa-node avec `pkill -f massa-node` et il suffit de relancer comme vous savez le faire.
    2. **massa-node** n'est pas à jour (Merci @mandawan0x sur le Discord Massa #french pour le retour)
8. **WARN massa_bootstrap::client: Error while connecting to bootstrap server: io error: Network is unreachable (os error 101)** : vous avez sûrement une IPv4 et votre node tente un bootstrap sur un node officiel avec une IPv6. Une IPv4 ne peut contacter que les IPv4 (Voir **Notes**). On peut ajouter ces lignes dans le fichier **massa/massa-node/config/config.toml** :

```toml
[bootstrap]
    # force the bootstrap protocol to use: "IPv4", "IPv6", or "Both". Defaults to using both protocols.
    bootstrap_protocol = "IPv4"
 ```

9. **WARN massa_bootstrap::client: Error while bootstrapping: general bootstrap error: Parsing Error: Failed BootstrapServerMessage deserialization / Failed MIP store deserialization / Failed MipStoreRaw der / Failed mip store stats der / Failed MipStoreStats network version counters der / Failed counters len der / Fail / Input: [15, 0, 15]** : Pour le moment, je ne sais pas d'ou viens le problème mais il devrait être résolu dans la version testnet 22.

10. **Error received from bootstrap server: IP ....... is not in the whitelist** : L'IP de votre node n'est pas autorisé à faire un bootstrap sur ce serveur. Il faut ajouter votre IP sur le serveur depuis `massa-client` avec :
```sh
node_bootstrap_whitelist add IPdeVotreNode
```

11. **Error while connecting to bootstrap server: all io errors except for Timedout, and would-block (unix error when timed out)** : Le problème est dans le fichier `massa/massa-node/config/config.toml`. Il vous faut le vérifier. Attention au changement de `[network]` en `[protocol]` à partir de l'épisode 22. Il faut ajouter une ligne vide à la fin de ce fichier aussi.

12. **Error while connecting to bootstrap server: Bootstrap IO error: Address family not supported by protocol (os error 97)** : vous avez désactivé la gestion de l'IPv6 dans votre OS et vous ne pouvez pas vous connecter avec l'IPv6 d'un node de boostrap.

13. **Error while connecting to bootstrap server: io error: Cannot assign requested address (os error 99)** : Pour le moment, je ne sais pas d'ou viens le problème.

## Notes
1. Si vous êtes en IPv4, vous ne pouvez pas faire un bootstrap sur un node cible en IPv6, seulement avec une IPv4
2. Si vous êtes en IPv6, vous pouvez faire un bootstrap sur des node en IPv6 ou IPv4
