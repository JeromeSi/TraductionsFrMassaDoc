# Mise à jour d'un nœud

Si vous utilisez les binaires, il faut simplement télécharger la nouvelle version.

Si vous utilisez le code source, télécharger la version nightly-2022-11-14 de Rust :

    rustup install nightly-2022-11-14

Utiliser par défaut la dernière version quotidienne :

    rustup default nightly-2022-11-14

Ensuite :

Assurez vous d'avoir le bon dépôt git (surtout lors du changement de GitLab à GitHub, N.d.T. remarque obsolète) :

    cd massa/
    git stash
    git remote set-url origin https://github.com/massalabs/massa.git

Mise à jour de Massa:

    git checkout testnet
    git pull

Après la mise à jour, utiliser la commande `node_get_staking_addresses` dans votre client et vérifier que l'adresse envoyée est bien celle qui contient les jetons ("rolls") avec `wallet_info`.

- Si `wallet_info` renvoie aucune adresse, cela signifie que vous n'avez pas sauvegardé votre fichier `wallet.dat` , démarrer le client et essayer à nouveau. Vous pouvez aussi créer une nouvelle adresse en utilisant  `wallet_generate_secret_key`.

- Si vous ne trouvez pas d'adresse avec `wallet_info` qui n'a pas 0 candidate, 0 final et 0 active rolls, veuillez vous référer [staking tutorial](https://massa.readthedocs.io/en/latest/testnet/staking.html) [Fr](./Staking.md) pour obtenir des jetons ("rolls").

- Si `node_get_staking_addresses` ne renvoie aucune adresse ou une adresse qui ne correspond pas à celle de `wallet_info` qui contient des jetons, cela signifie que vous n'avez pas réutilisé le fichier `staking_keys.json` correctement. Essayer d'arrêter le nœud, de copier à nouveau le fichier `staking_keys.json` et démarrer à nouveau le nœud. Vous pouvez aussi ajouter manuellement la clé de *staking* en utilisant la commande `node_add_staking_secret_keys laclesecrete` avec la clé secrète qui possède des jetons ("rolls") dans `wallet_info` (attention : ne pas mettre l'adresse ou la clé publique, uniquement la clé secrète ("SECRET KEY")).
  
  Suivant : [Création d'un portefeuille Massa](./Creating_a_massa_wallet.md) / [Creating a Massa wallet](https://docs.massa.net/en/latest/testnet/wallet.html)
  
  Précédent : [Démarrer un nœud](./Running_a_node.md) / [Running a node](https://docs.massa.net/en/latest/testnet/running.html)
