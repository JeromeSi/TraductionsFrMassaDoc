# Démarrer un nœud

Traduction de “[Running a node](https://docs.massa.net/en/latest/testnet/running.html)”

## Depuis les binaires
Vous devez éxecuter le binaire que vous avez téléchargé puis décompresser à l'étape précédente : 
+ rendez-vous dans le dossier *massa/massa-node* et lancer l'éxecutable *massa-node*
+ rendez-vous dans le dossier *massa/massa-client* et lancer l'éxecutable *massa-client*

### Avec Ubuntu / MacOS

#### Configuration du noeud
La configuration par défaut est disponible [ici](https://github.com/massalabs/massa/blob/main/massa-node/base_config/config.toml).

Vous pouvez modifier cette configuration par défaut en créant le fichier *massa-node/config/config.toml*.

#### Démarrer le noeud

Dans une première fenêtre :
```sh
cd massa/massa-node/
```

Démarrage du node, avec Ubuntu :

```sh
./massa-node -p VotreMotDePasse |& tee logs.txt
```

Il faut remplacer `VotreMotDePasse` par un mot de passe que vous aurez choisi et qui sera demandé à chaque fois que vous voudrez démarrer le noeud.

#### Démarrer le client

Dans une seconde fenêtre :

```sh
cd massa/massa-client/
```

Démarrage du client, avec Ubuntu :

```sh
./massa-client -p VotreMotDePasse
```

Il faut remplacer `VotreMotDePasse` par un mot de passe que vous aurez choisi et qui sera demandé à chaque fois que vous voudrez démarrer le client.


## Depuis le code source

### Avec Ubuntu / MacOS

#### Démarrer le nœud

Dans un premier terminal :

```sh
cd massa/massa-node/
```

Compiler (si nécessaire) et démarrer le nœud avec Ubuntu / Debian

```sh
RUST_BACKTRACE=full cargo run --release -- -p VotreMotdePasse |& tee logs.txt
```

Remplacer `VotreMotdePasse` avec un mot de passe que vous devez conserver pour redémarrer votre nœud. Veuillez attendre que la compilation de massa-node se termine avant de continuer.

Ou, avec macOS :

```sh
RUST_BACKTRACE=full cargo run --release -- -p VotreMotdePasse > logs.txt 2>&1
```

Remplacer `VotreMotdePasse` avec un mot de passe que vous devez conserver pour redémarrer votre nœud.

#### Démarrer le client

Dans un second terminal :

```sh
cd massa/massa-client/
```

Puis :

```sh
cargo run --release -- -p VotreMotdePasse
```

Remplacer `VotreMotdePasse` avec un mot de passe que vous devez conserver pour redémarrer votre client. Veuillez attendre que la compilation de massa-client se termine avant de continuer.

### Avec Windows

#### Démarrer le nœud

+ Ouvrir Windows Power Shell ou Command Prompt dans une première fenêtre
  + Écrire : `cd massa`
  + Écrire : `cd massa-node`
  + Écrire : `cargo run --release -- -p VotreMotdePasse`

Remplacer `VotreMotdePasse` avec un mot de passe que vous devez conserver pour redémarrer votre nœud. Veuillez attendre que la compilation de massa-node se termine avant de continuer.

#### Démarrer le Client

+ Ouvrir Windows Power Shell ou Command Prompt dans une seconde fenêtre
  + Écrire : `cd massa`
  + Écrire : `cd massa-client`
  + Écrire : `cargo run --release -- -p VotreMotdePasse`

Remplacer `VotreMotdePasse` avec un mot de passe que vous devez conserver pour redémarrer votre client. Veuillez attendre que la compilation de massa-client se termine avant de continuer.

|**ATTENTION !**|
|---------------|
|Si vous avez un cas de crash à la compilation avec Rust ou lors du fonctionnement, n'ouvrez pas un rapport de bug sur le dépôt rustlang/rust mais sur le dépot massa à la place. Nous ferons le tri des problèmes et ouvrirons un rapport de bug si il y a lieu. Cela permet de ne pas encombrer le dépôt principal de rust avec des rapports concernant le même bug.|

Suivant : [Mise à jour d'un nœud](./Update.md) / [Update a node](https://docs.massa.net/en/latest/testnet/update.html)

Précédent : [Installation d’un nœud](./Installing_a_node.md) / [Installing a node](https://docs.massa.net/en/latest/testnet/install.html)
