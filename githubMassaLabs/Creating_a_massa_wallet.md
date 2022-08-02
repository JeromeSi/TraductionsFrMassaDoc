# Création d’un portefeuille Massa

Traduction de “[Creating a Massa wallet](https://docs.massa.net/en/latest/testnet/wallet.html)”

Comme les autres blockchains, Massa utilises la cryptographie à courbe elliptique pour la sécurité des jetons (avec secp256k1).

Cela signifie que votre clé secrete (“secret key”) est votre mot de passe vous autorisant à dépenser des jetons que vous avez sur votre adresse (votre adresse est un hash de votre clé publique).

Maintenant, nous allons créer un portefeuille Massa.

## À partir de l’interface en ligne de commande

### Si votre client ne fonctionne pas

Aller dans le dossier du client depuis une fenêtre de terminal :

`cd massa/massa-client/`

Démarrer le client intéractif and charger le fichier de portefeuille (`wallet.dat`) :

`cargo run`

Le client vous demande le mot de passe pour charger le fichier `wallet.dat`. Si le fichier `wallet.dat` n’éxiste pas, vous devrez saisir un mot de passe que vous devrez conserver et le `wallet.dat` est créé.
Si votre client fonctionne

Maintenant, il faut générer une nouvelle clé d’appairage (et l’associer à une adresse) :

`wallet_generate_secret_key`

**Ou, si vous en avez déjà une venant d’un précédent wallet**, vous pouvez ajouter manuellement une clé d’appairage :

`wallet_add_keys <your_key>`

On obtient la liste des clé de votre portefeuille accessible avec :

`wallet_info`

## Avec l’interface graphique

Si vous ne souhaitez pas participer à la production de blocs (“staking”) ou utiliser la ligne de commande, vous pouvez créer un portefeuille à partir de l’interface web : en haut à droite de [l’explorateur](https://massa.net/testnet/), dérouler le menu et choisir [“Wallet”](https://massa.net/testnet/wallet).

Cliquer sur le bouton “Generate” puis sur le bouton “Add” (ajouter).

Cette action permet de générer une cl d’appairage aléatoire depuis le générateur de nombre aléatoire de votre ordinateur, il n’est pas transférer sur le réseau.

Maintenant, vous pouvez ajouter d’autres addresses ou voir la liste de vos adresses avec leur numéro de “thread” et le solde.

Vous avez la possibilité d’enregistrer ce wallet et de le récupérer plus tard en sauvegardant la clé privé avec le bouton “Save” (N.d.T. : Je ne vois pas sur l’interface…).
Vous pouvez utiliser un portefeuille précédemment créer en chargeant le fichier `wallet.dat` précédemment créer : bouton “Load wallet(.dat file)”.

Suivant : [Mise à jour d'un nœud](./Update.md) / [Update a node &mdash; Massa documentation](https://docs.massa.net/en/latest/testnet/update.html)

Précédent : [Démarrer un node](./Running_a_node.md) / [Running a node](https://docs.massa.net/en/latest/testnet/running.html)
