# Questions fréquentes

Traduction de "[Frequently Asked Questions](https://docs.massa.net/en/latest/testnet/faq.html)"

## Questions d'ordre général

-----------------------------------

### Quel est la configuration minimale ?

La philosophie de Massa est d'être la plus décentralisée possible. Pour atteindre cet objectif, nous visons la configuration la plus modeste possible pour que la plupart des personnes puissent participer. Actuellement, 4 cœurs et 8 GB de RAM sont suffisants pour démarrer un nœud. Comme le taux de transactions va augmenter cela ne sera pas suffisant. Finalement, la configuration visée sera 8 cœurs, 16 GB de RAM et 1TB de disque.

--------------------

### Est-ce que ça marche avec un VPS ?

Vous pouvez utilisez un VPS pour faire fonctionner un nœud. Les avantages d'un VPS est que l'on a une haute disponibilité et qu'il est facile à configurer. Par contre, cela signifie que les nœuds fonctionnant sur un VPS favorise la centralisation si les nœuds fonctionnent sur le même fournisseur (p.e. AWS).

### Comment faire pour mon noeud continue de fonctionner quand je ferme le terminal ?

-------------------------------------------------------

Vous pouvez utilisez la commande suivante pour lancer votre nœud :

`nohup cargo run --release &`

Les messages du nœud iront dans le fichier `nohup.out`. Vous pourrez fermer votre terminal sans risque de couper le nœud. Pour terminer l'application vous pouvez utilisez :
`pkill -f massa-node`

Vous pouvez aussi utiliser la commande [`screen`](https://help.ubuntu.com/community/Screen) ou [`tmux`](http://manpages.ubuntu.com/manpages/cosmic/man1/tmux.1.html) par exemple.

### Est-ce que Massa incluera les smart contrats ?

-----------------------------------

Nous essayons d'inclure, à la fois, les EVM pour la rétro compatibilité et un moteur pour des smart contracts spécifiques utilisant pleinement le protocole Massa. Nous souhaitons aussi les développer pour des langages plus utilisés et introduire quelques innovations.

Nous finissons l'implémentation de la première version du moteur de smart contract qui sera intégré prochainement. (N.d.T. : c'est fait !) 

Nous avons prévu quelques fonctionnalités intéressantes, comme le réveil autonome, un peu comme ce qui est annoncé [ici](https://arxiv.org/pdf/2102.10784.pdf).

### Quels sont les port utilisés par Massa ?

--------------------------

Par défaut, Massa utilise le TCP du port 31244 pour le protocole de communication avec les autres nœuds et le TCP du port 31245 pour se connecter ("bootstrap") aux autres nœuds. Massa utilises aussi le port 33034 pour une nouvelle API privée et le 33035 pour la nouvelle API publique (API v2).

### Comment démarre-t-on un nœud ?

------------------------

- Ubuntu : `ctrl` + `c` pour terminer le précédent processus de nœud et `cargo run --release |& tee logs.txt` pour le  démarrer
- Windows : `ctrl` + `c` pour terminer le précédent processus de nœud et `cargo run --release` pour le démarrer
- Mac Os : `ctrl` + `c` pour terminer le précédent processus de nœud et `cargo run --release > logs.txt 2>&1` pour le  démarrer

### Comment peut-on faire pour sécuriser les clefs d'appairage ?

--------------------------------

Veuillez bien noter que  que la monnaie du testnet n'a AUCUNE valeur. Ceci dit, nous travaillons pour ajouter du cryptage à plusieurs niveaux avant le Mainnet.

Le fichier de clé de staking  dans le dossier du nœud et le fichier du portefeuille dans le dossier du client ne sont actuellement pas cryptés mais cela arrivera bientôt. De même, l'API de communication entre le client et le nœud n'est pas cryptée pour le moment mais cela sera fait pour le Mainnet.

Notez que les nœuds e connaissent et ne font pas confiance aux autres et n'échangent jamais d'informations sensibles donc le cryptage n'est pas nécessaire à ce niveau.
Une vérification ("handshake") est faite à la connexion avec un autre pair. Nous signons des données aléatoires que le pair a transmis avec notre clé d'appairage et la même chose pour l'autre pair. Les données sont signé par le créateur et pas par le nœud qui les envoie.
Durant la connexion au réseau Massa ("bootstrap"), la vérification est asymétrique. Nous connaissons la clé publique du nœud de *bootstrap* et nous attendons des messages signés de lui mais nous ne communiquons pas notre clé publique ni nous ne signons le seul message que nous envoyons (uniquement des données aléatoires).

## Solde et portefeuille

### Comment peut-on changer de serveur sans perdre le portefeuille ?

-----------------------------------------------------------------------------------

Vous avez besoin de conserver le fichier `wallet.dat` et de le copier dans le dossier `massa-client` du nouveau serveur. Vous pouvez aussi conserver et copier le fichier `node_privkey.key` dans le dossier `massa-node/config` pour conserver vos statistiques de connexion.

Si vous avez des jetons ("rolls"), vous devez aussi conserver l'adresse pour acheter les jetons pour recommencer le *staking* (voir [Staking](https://docs.massa.net/en/latest/testnet/staking.html) [[FR]](./Staking.md)).

### Pourquoi mes soldes dans le client et l'explorateur sont différents ?

---------------------------------------------------------------

Cela peut vouloir dire que votre nœud est désynchronisé.
Vérifier que votre nœud fonctionne, que la machine correspond à la configuration requise et essayer de redémarrer votre nœud.

### Est-ce que la commande `cargo run -- --wallet wallet.dat` écrase mon portefeuille existant ?

--------------------------------------------------------------------------------

Non, cela charge le portefeuille s'il existe sinon ça le crée.

### Ou se trouve le fichier `wallet.dat` ?

--------------------------------

Par défaut le fichiet est dans le dossier `massa-client`.

## Jetons ("Rolls") et *staking*

### Mes jetons disparaissent ou sont vendus automatiquement.

---------------------------------------------

La raison la plus probable est que le nœud n'a pas produit les blocs quand il a été sélectionné pour le faire. Les raisons les plus fréquentes :

- Le nœud n'a pas fonctionné 100% du temps durant le quel vous aviez au moins un jeton actif ("active_rolls")
- Le nœud n'a pas été correctement connecté au réseau 100% du temps durant le quel vous aviez au moins un jeton actif ("active_rolls")
- Le nœud a été désynchronisé (ce qui peut se produire par une surcharge temporaire si la configuration matérielle est insuffisante ou si d'autres programme utilisent les ressources sur la machine ou parce que la connexion Internet est défaillante/insuffisante)
- Le nœud  n'a pas la bonne adresse de *staking* (utiliser `node_get_staking_addresses` dans le client pour vérifier qu'elle correpond à celle de votre portefeuille qui détient les jetons actifs ("active_rolls")) 100% du temps durant le quel vous aviez au moins un jeton actif ("active_rolls")
- Certains fournisseurs (N.d.T. de VPS) ont des connexions Half-duplex. Contacter le support du fournisseur pour obtenir une connexion full-duplex.

Protocole de diagnostique :

- vérifier que votre nœud fonctionne sur votre machine dont les caractéristiques correspondent à la configuration requise et qu'aucun autre programme n'utilise les ressources
- utiliser `wallet_info` et vérifier qu'au moins une des adresses possède au moins un jeton actif ("active rolls")
  - si  il n'y a aucune adresse, il faut en créer une [voir ici](https://docs.massa.net/en/latest/testnet/wallet.html) [Fr](./Creating_a_massa_wallet.md) et recommencer le diagnostique
  - si aucune adresse listée possède de jetons actif ("active rolls"), faire un achat de jeton [voir ici](https://docs.massa.net/en/latest/testnet/staking.html) [Fr](./Staking.md) et recommencer le diagnostique
- Utiliser `node_get_staking_addresses` dans le client :
  - si la liste est vide ou si aucune adresse affichée ne correspond à celle affichée avec `wallet_info` qui possède au moins un jeton actif ("active rolls") 
    - utiliser la commande `node_add_staking_secret_keys` avec la clé secrète qui correspond à l'adresse qui possède au moins un jeton actif affiché par `wallet_info`
- Vérifier votre adresse avec [l'explorateur en ligne](https://massa.net/testnet/) : si il y a une différence entre le nombre de jetons actifs affiché en ligne et celui retourné par `wallet_info` du client, cela devrait indiquer que votre nœud est désynchronisé. Essayer de le redémarrer.

### Pourquoi mes jetons sont vendus automatiquement ? Est-ce une sorte de pénalité/amende ?

----------------------------------------------------------------------

Ce n'est pas une amende car les fonds sont intégralement reversés. C'est plus une vente automatique de jetons.

La raison est la suivante : pour la santé du réseau, chacun en fonction du nombre de jetons actifs doit produire des blocs quand il est sélectionné. Si une adresse échoue à plus de 70% de sa création de blocs durant le cycle C, tous les jetons actifs sont automatiquement vendus au début du cycle C+3.

### Dois-je enregistrer mes clée après avoir acheter des jetons ("ROLLs") ou ils sont *stakés* automatiquement ?

--------------------------------------------------------------------------------------------------------

Actuellement, ils ne vont pas *staker* automatiquement. Dans les prochaines version, nous ajouterons une fonctionnalité ou cela se fera automatiquement. Ceci dit, certaines personnes ont l'air de l'avoir fait très tôt dans le projet. N'hésitez pas à poser des questions sur le [Discord](https://discord.com/invite/massa). (N.d.T. actuellement, on s'enregistre une seule fois pour commencer le staking)

### Je peux acheter, envoyer et vendre mes jetons ("ROLLs") et ma monnaie sans frais. Quand devrais-je mettre des frais supérieure à 0 ?

---------------------------------------------------------------------------------------

Pour le moment, il y a peu de transaction en même temps et la plupart des blocs créés sont vides. Cela signifie que votre opération sera ajoutée à un bloc même si les frais sont nuls. Nous vous indiquerons si vous avez besoin d'augmenter les frais.

J'ai des jetons ("ROLLs") mais les informations de mon portefeuille ne change pas. Quand aurais-je mes premiers revenus ?
---------------------------------------------------------------------------------------------

Vous devez attendre que vos jetons passent actifs ("Active rolls") environ 1h45min maximum alors, en fonction de votre nombre de jetons actifs ("Active rolls"), vous devrez attendre aussi d'être sélectionné pour des blocs et de les produire.

Testnet et récompenses
===================

Comment foire pour déplacer mon nœud d'une machine à une autre et conserver mon score du programme de récompenses du Testnet ?
------------------------------------------------------------------------------------------------------------------------

Si vous déplacez votre nœud d'une machine à une autre, vous devez conserver votre clé d'appairage avec adresse de *staking* qui est enregistrée. La clé d'appairage est dans le fichier "wallet.dat"  dans le dossier "massa-client". Vous pouvez aussi conserver la clé d'appairage de votre nœud situé dans le fichier "node_privkey.key" dans le dossier "massa-node/config/". Si vous ne faites pas ces sauvegardes et restauration, il faut vous enregistrer votre nœud sur le bot ("MassaBot") dans Discord.

Si votre nouveau nœud possède une nouvelle adresse IP, il faut l'envoyer au bot "MassaBot" de Discord (N.d.T. et modifier l'adresse IP du nœud présente dans le fichier "massa-node/config/config.toml").

Si vous perdez vos fichiers "wallet.dat" et "node_privkey.key", cela ne pose pas de problème car les récompenses sont liés au compte Discord. Vous devez refaire un nouveau nœud et l'enregistrer auprès de "MassaBot". Les anciens scores ne sont pas perdus.

Je veux faire plus de *staking* ! Peut-on abuser du faucet pour obtenir plus de monnaies ?
-------------------------------------------------------------------

Vous pouvez demander 100Mas toutes les 24h (N.d.T. remise à 0 à 00:00 UTC). Cette monnaie ne donne aucun avantage sur les autres en faisant cela.

Est-ce que le nombre de jetons *stakés* affecte la récompense du Testnet ?
-------------------------------------------------------

Non tant que vous avez au moins un jeton actif. Tous jetons supplémentaires ne changent pas votre score.

Je ne peux pas m'enregistrer auprès de MassaBot car le "node ID" est déjà utilisé.
-------------------------------------------------------------------------

Si vous avez changé d'adresse de *staking*, vous devez vous enregistrer à nouveau auprès de MassaBot en utilisant la commande `node_testnet_rewards_program_ownership_proof` dans le client.

Si vous utilisez la même installation, MassaBot retournera le message suivant :

`This node ID is already used or has already been used, please use another one!`

Pour résoudre le problème, vus devez générer un nouvel "Node ID". Vous devez arrêter votre nœud et effacer le fichier `massa-node/config/node_privkey.key`. Vous pouvez démarrer votre nœud et vous aurez un nouvel "Node ID".

Common issues
=============

Ping too high issue
-------------------

Check the quality of your internet connection. Try increasing the
"max_ping" setting in your config file:

- edit file `massa-node/config/config.toml` (create if it is absent) with the following
  content:
  
  .. code-block:: toml
  
      [bootstrap]
      max_ping = 10000 # try 10000 for example

API can't start
---------------

- If your API can't start, e.g. with
  `could not start API controller: ServerError(hyper::Error(Listen, Os { code: 98, kind: AddrInUse, message: "Address already in use" }))`,
  it's probably because the default API ports 33034/33035 are already in use
  on your computer. You should change the port in the config files,
  both in the API and Client:
* create/edit file `massa-node/config/config.toml` to change the port used by the API:
  
  .. code-block:: toml
  
      [api]
      bind_private = "127.0.0.1:33034" # change port here from 33034 to something else
      bind_public = "0.0.0.0:33035" # change port here from 33035 to something else
- create/edit file `massa-client/config/config.toml` and put the same
  port:
  
  .. code-block:: toml
  
      [default_node]
      ip = "127.0.0.1"
      private_port = 33034 # change port here from 33034 to the port chosen in node's bind_private
      public_port = 33035 # change port here from 33035 to the port chosen in node's bind_public

Raspberry Pi problem "Thread 'main' panicked"
---------------------------------------------

If you encountered an error message such as:

"Thread 'main' panicked at 'called Option::unwrap() on a None value', models/src/hasher.rs:35:46", this is a known problem on older Raspberry Pi,
especially with Raspbian. Try installing Debian.

Please note, running a Massa node on a Raspberry Pi is ambitious and will probably not work that well. We don't
expect raspberry to be enough powerful to run on the mainnet.

Disable IPV6 support
--------------------

If your OS, virtual machine or provider does not support IPV6, try disabling IPV6 support on your Massa node.

To do this, edit (or create if absent) the file `massa-node/config/config.toml` with the following contents:

    .. code-block:: toml
    
        [network]
            bind = "0.0.0.0:31244"
    
        [bootstrap]
            bind = "0.0.0.0:31245"

then restart your node.
