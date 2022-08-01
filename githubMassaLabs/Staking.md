# Staking

Traduction de [“Staking](https://docs.massa.net/en/latest/testnet/staking.html)”

Dans Massa, le nombre minimal de monnaies requis pour produire (“to stake”) est de 100 MAS (ce qui est appelé un “roll”, jeton). L’offre de départ est de 500 millions de MAS, en théorie, il pourrait y avoir 5 millions de personnes qui vont produire.

Les adresses qui vont produire (“stake”, créer un bloc) sont sélectionnés au hasard dans tous les “threads” basé sur le nombre de “roll” possédés. La liste des stakers (producteurs) avec le nombre de rolls est [accessible à cette adresse](https://massa.net/testnet/staking/).

Les rolls peuvent être achetés avec la monnaie de Massa ou vendu pour retrouver cette monnaie. Si vous avez au moins 100 MAS, vous pouvez continuer ce tutoriel. Dans le cas contraire, il vous faut envoyer votre adresse (“address” obtenu avec “wallet_info” dans le client Massa) au bot dans le canal “testnet-faucet” de notre [Discord](https://discord.com/invite/massa).
## Acheter des rolls

Il faut aller chercher l’adresse qui possède des MAS dans votre portefeuille. Dans le client Massa, on utilise :

`wallet_info`

On achète des rolls avec “buy_rolls” auquel on ajoute l’adresse (“Address” de “wallet_info”), le nombre de rolls que l’on veut et les frais (“fee”) que l’on paie (on peut mettre 0) :

`buy_rolls <address> <roll count> <fee>`

Par exemple, la commande achetant `1` roll avec `0` de frais pour l’adresse `VkUQ5MA4niNBhAEP7uVf89tvPfUHcbgy6BrdLM9SAuFSyy9DE` est :

`buy_rolls VkUQ5MA4niNBhAEP7uVf89tvPfUHcbgy6BrdLM9SAuFSyy9DE 1 0`

Cela devrait prendre moins d’une minute pour vos rolls parviennent à l’étape finale (“final”), on vérifie avec :

`wallet_info`

N.d.T. : Vous devez lire la suite pour que cela fonctionne.

## Indiquer que votre nœud peut commencer le “staking” avec vos “rolls”

On récupère l’adresse secrète du portefeuille qui contient vos “rolls” :

`wallet_info`

On enregistre l’adresse secrète comment pouvant faire du ”staking” avec la commande :

`node_add_staking_secret_keys <your_secret_key>`

Maintenant, vous devais attendre que vos rolls deviennent actif (“active”) au bout de 3 cycles de 128 périodes (une période produit 32 blocs — 16s), environ 1h 40min

Vous pouvez vérifier si vous avez des rolls actifs avec la commande :

`wallet_info`

Quand vos rolls deviennent actifs, c’est bon ! Vous faites du staking ! Veuillez noter qu’un seul roll actif suffit. Sur le testnet, nous ne valorisons pas le nombre de rolls actifs que vous possédez mais la fiabilité de votre nœud.

Vous serez sélectionner pour créer des blocs dans les différents threads.

Pour vérifier quand votre adresse est sélectionnée pour faire un bloc, vous pouvez utiliser cette commande dans le client :

`get_addresses <your_address>`

il faut regarder dans la section “Block draws”.

Vous pouvez aussi vérifier que, dans la section “Sequential balance:”, “Final balance” et “Candidate balance” augmentent à chaque bloc produit d’un petit montant.
## Vendre des rolls

Si vous voulez récupérer une partie des MAS ou la totalité, vous devez vendre une partie ou tous vos rolls en suivant le même principe que pour l’achat :

`sell_rolls <address> <roll count> <fee>`

Cela prendra du temps avant que vos MAS soit à nouveau disponible et ils seront gelés (“Locked balance”) pendant 1 cycle avant que vous puissiez les dépenser. La vérification se fait avec :

`wallet_info`

Suivant : [Accessibilité](Routability.md) / [Routability](https://docs.massa.net/en/latest/testnet/routability.html)

Précédent : [Création d’un portefeuille Massa](./Creating_a_massa_wallet.md) / [Creating a Massa wallet](https://docs.massa.net/en/latest/testnet/wallet.html)
