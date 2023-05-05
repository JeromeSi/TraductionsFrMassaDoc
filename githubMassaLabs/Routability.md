# Accessibilité

Traduction de “[Routability](https://docs.massa.net/en/latest/testnet/routability.html)"

## Principe

Les nœuds dans le réseau Massa doivent établir des connexions entre eux pour communiquer, propager les blocs, les opérations, maintenir le consensus et être tous synchronisés.

Pour qu’un nœud A établisse une connexion avec le nœud B, le nœud B doit être accessible (“routable”). Cela signifie que le nœud B doit avoir une adresse IP publique qui peut-être atteint par le nœud A, que les ports 31244 et 31245 soient ouvert sur le nœud B et que les connexions entrantes soient autorisées par le pare-feu (“firewall”) sur le nœud B. Une fois que la connexion est établie, la communication à travers cette connexion est bidirectionnelle et cela n’a aucune importance de connaître le nœud qui est à l’initiative de la connexion.

Si seulement un petit nombre de nœuds est accessible, tous les autres nœuds ne pourront qu’à ces nœuds accessibles ce qui provoquera une surcharge de ceux-ci et, généralement, diminuera la décentralisation et la sécurité du réseau. Ce petits nombre de nœuds accessibles deviendra de-facto un hub central de communication, un point de ralentissement avec un fort risque d’erreurs. Il est important qu’il y ait le plus grand nombre de nœuds accessibles possibles.

Avec Massa, les nœuds ne sont pas accessible par défaut et requiert une intervention humaine pour être accessible.

## Comment rendre mon nœud accessible ?

+ Vérifier que l’ordinateur qui servira de nœud dispose d’une IP publique fixe (IPv4 ou IPv6). Vous pouvez connaître votre IP publique avec avec le site [ipify](https://api.ipify.org/) (N.d.T. : en IPv4, [ipv6-test](https://ipv6-test.com/) fonctionne en IPv6 et IPv4)
+ Si l’ordinateur de votre nœud est derrière un routeur/NAT, vous devez le configurer :
  + si routeur utilise le DHCP, vous pouvez configurer votre routeur qu’il attribue toujours la même IP à l’ordinateur du nœud en utilisant l’adresse MAC (une adresse IP local qui ne change pas, en général du genre 192.168.X.XXX)
  + sur le routeur, les connexions entrantes TCP des ports 31244 et 31245 doivent être redirigées vers l’adresse IP local de l’ordinateur du nœud
+ Modifier la configuration du parefeu pour autoriser les connexions entrantes TCP sur les ports 31244 et 31245 (exemple : `sudo ufw allow 31244 && sudo ufw allow 31245` avec Ubuntu / Debian, ou configurer Windows Firewall on Windows)
+ Il faut éditer le fichier `massa-node/config/config.toml` (ou le créer s’il n’éxiste pas) avec le contenu suivant 
```toml
[protocol]
  routable_ip = "AAA.BBB.CCC.DDD"
```
  ou AAA.BBB.CCC.DDD doit être remplacé par votre adresse IP publique (pas l’adresse local). IPv6 fonctionne aussi.
+ Vous devez redémarrer le nœud pour que les changements dans r massa-node/config/config.toml soient pris en compte.
+ Vous pouvez tester si vos ports sont bien ouvert en écrivant votre adresse IP publique et le port 31244 dans le site [yougetsignal](https://www.yougetsignal.com/tools/open-ports/) (même chose pour le port 31245)
+ Une fois que votre noeud est accessible, vous devez envoyé votre adresse IP publique au bot Discord MassaBot en conversation privée. En premier, vous devez vous enregistrer pour le programme de récompense (c’est juste en dessous)

## Dernière étape

+ Pour valider votre participation au programme de récompense du testnet, vous devez vous enregistrer avec votre compte Discord. Écrire quelque chose dans le canal *testnet-rewards-registration* de notre [Discord](https://discord.com/invite/massa) et notre bot MassaBot vous enverra les instruction par conversation privée. Plus d’information ici : [Testnet rewards program](https://massa.readthedocs.io/en/latest/testnet/rewards.html).

Suivant : [Programme de récompense du testnet](./rewards.md)/ [Testnet rewards program](https://massa.readthedocs.io/en/latest/testnet/rewards.html)

Précédent : [Participation](./Staking.md) / [Staking](https://docs.massa.net/en/latest/testnet/staking.html)
