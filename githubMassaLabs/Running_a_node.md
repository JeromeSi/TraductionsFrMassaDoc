# Démarrer un nœud

Traduction de “[Running a node](https://docs.massa.net/en/latest/testnet/running.html)”

## Depuis les binaires

Il faut juste démarrer les binaires que vous avez télécharger précédemment en utilisant les étapes suivantes :

+ Aller dans le dossier `massa/massa-node` et démarrer l’exécutable `massa-node` qui s’y trouve
+ Aller dans le dossier `massa/massa-client` et démarrer l’exécutable `massa-client` qui s’y trouve

## Depuis le code source

### Avec Ubuntu / MacOS

#### Démarrer le nœud

Dans un premier terminal :

`cd massa/massa-node/`

Compiler (si nécessaire) et démarrer le nœud avec Ubuntu / Debian

`RUST_BACKTRACE=full cargo run --release -- -p <PASSWORD> |& tee logs.txt`

Remplacer <PASSWORD> avec un mot de passe que vous devez conserver pour redémarrer votre nœud. Veuillez attendre que la compilation de massa-node se termine avant de continuer.

Ou, avec macOS :

`RUST_BACKTRACE=full cargo run --release -- -p <PASSWORD> > logs.txt 2>&1`

Remplacer <PASSWORD> avec un mot de passe que vous devez conserver pour redémarrer votre nœud.

#### Démarrer le client

Dans un second terminal :

`cd massa/massa-client/`

Puis :

`cargo run --release -- -p <PASSWORD>`

Remplacer `<PASSWORD>` avec un mot de passe que vous devez conserver pour redémarrer votre client. Veuillez attendre que la compilation de massa-client se termine avant de continuer.

### Avec Windows

#### Démarrer le nœud

+ Ouvrir Windows Power Shell ou Command Prompt dans une première fenêtre
  + Écrire : `cd massa`
  + Écrire : `cd massa-node`
  + Écrire : `cargo run --release -- -p <PASSWORD>`

Remplacer `<PASSWORD>` avec un mot de passe que vous devez conserver pour redémarrer votre nœud. Veuillez attendre que la compilation de massa-node se termine avant de continuer.

#### Démarrer le Client

+ Ouvrir Windows Power Shell ou Command Prompt dans une seconde fenêtre
  + Écrire : `cd massa`
  + Écrire : `cd massa-client`
  + Écrire : `cargo run --release -- -p <PASSWORD>`

Remplacer `<PASSWORD>` avec un mot de passe que vous devez conserver pour redémarrer votre client. Veuillez attendre que la compilation de massa-client se termine avant de continuer.

Suivant : [Mise à jour d'un nœud](./Update.md) / [Update a node](https://docs.massa.net/en/latest/testnet/update.html)

Précédent : [Installation d’un nœud](./Installing_a_node.md) / [Installing a node](https://docs.massa.net/en/latest/testnet/install.html)
