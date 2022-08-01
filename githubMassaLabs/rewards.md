# Programme de récompense pour la participation eu testnet

Traduction de “[Testnet Staking Rewards Program](https://docs.massa.net/en/latest/testnet/rewards.html)”

Pour nous aider à parvenir à notre objectif d’une blockchain complètement décentralisée et répartie, nous avons mis au point un programme de récompense pour la participation durant la phase de test réseau (“testnet”).

Ceux qui font fonctionner un nœud et produises des blocs seront récompensés par de la monnaie de Massa durant la phase de fonctionnement (“mainnet”).

La participation (“staking”) est ce qui améliore la sécurité du réseau. En achetant des rolls (jetons), on immobilise de la monnaie Massa et on participe à la production de blocs. Vous aidez les nœuds honnêtes à se protéger collectivement contre les attaquants potentiels qui doivent atteindre 51% de la production de blocs. Sur le mainnet, le staking est encouragé par l’obtention de récompense lors de la production de bloc, pour chaque bloc produit vous obtenez quelques MAS. Sur le testnet, vous obtenez aussi des MAS qui n’ont aucune valeur, c’est pourquoi nous vous récompenserons lors du mainnet pour avoir appris à faire fonctionner un nœud et avoir participer à la production de blocs (“staking”) ce qui nous aide à améliorer l’expérience utilisateur de staking.

Le 16 juillet 2021, nous avons lancé la première version publique de Massa, le premier testnet. Plus de 350 nœuds étaient connectés en même temps après la première semaine, ce qui a surchargé nos nœuds de démarrage qui étaient les seuls nœuds qui acceptaient les connexions. En paramétrant votre nœud pour qu’il soit accessible (avec une adresse IP publique), vous devenez un véritable pair (“peer”) et un réseau pair-à-pair (“peer-to-peer”) : vous ne vous connectez pas seulement à des nœuds existant mais vous offrez aussi la possibilité à d’autres personnes la possibilité d’accéder au réseau à travers votre connexion. Il y a aussi une récompense qui sera fonction du nombre de fois que votre nœud sera accessible.

## Épisodes

Nous faisons des cycle d’un mois chacun, appelés “épisodes”, le premier a démarré en Auout 2021. Au début d’un épisode, les participants ont quelques jours pour paramétrer leur nœud avec la nouvelle version avant que le score ne commence mais il aussi possible de rejoindre à n’importe quel moment l’épisode en cours.

Durant l’épisode, vous pouvez demander de la monnaie Massa sur le Discord (dans le canal “testnet-faucet”). Inutile d’abuser du faucet, la récompense n’est pas fonction du nombre de rolls actifs.

À la fin d’un épisode, nous arrêtons tous les noeuds et ils deviennent inutilisables. Les participants doivent télécharger et démarrer la nouvelle version pour l’épisode suivant. Faites attention de conserver le la même clé privée et le même portefeuille ! (N.d.T. : cette dernière recommandation n’est plus d’actualité).

##Formule utilisée pour le score

Le score pour un noeud pour un épisode donné est :

| `Score = 50 * (active_cycles / nb_cycles) * (produced_blocks / selected_slots) + 50 * (routable_cycles / nb_cycles) + 20 * total_maxim_factor / nb_cycles`

+ 50 points du score est basé sur le staking :
    + `(active_cycles / nb_cycles ) * (produced_blocks / selected_slots)`
        + `active_cycles` est le nombre de cycles durant l”épisode ou le noeud possede au moins un roll actif.
        + `nb_cycles` est le nombre total de cycle dans l’épisode.
        + `produced_blocks` est le nombre de bloc produit par le nœud durant l’épisode.
        + `selected_slots` est le nombre de fois ou le nœud a été sélectionné pour produire un bloc. Si `selected_slots = 0`, le score de staking est laissé à 0.
        +  Le score maximum devrait être atteint si, durant tout l’épisode, le nœud possède au moins un roll actif et a produit tous les blocs pour lesquels il a été sélectionné.
    + 50 points du score sont basés sur l’accessibilité du nœud : combien de fois le nœud a été accessible aux autres nœuds.
        + `routable_cycles / nb_cycles`
        + `routable_cycles` est le nombre de tentative de connexion qui ont réussi.
        + Le score maximum est atteint si le nœud a pu toujours être atteint par les autres nœuds.
    + 20 points du score mesure la répartition des nœuds : le réseau est plus décentralisé si les nœuds sont répartis entre les pays et FAI (Fournisseur d’Accès Internet) que s’ils sont tous hébergé dans en un même lieu avec le même FAI.
        + `total_maxim_factor / nb_cycles`
        + `total_maxim_factor` est la somme des `maxim_factor` de chaque cycle. Le `maxim_factor` est une valeur comprise entre 0 et 1 représentant la distance entre l’adresse IP du noeud et les adresses IP des autres noeud pour un cycle donné.
        + Le score maximum est atteint quand le nœud fonctionne à domicile ou avec un un FAI qui n’est pas utilisé par un autre nœud.

Nous demandons à tous les participants de ne faire fonctionner qu’un seul noeud. Les noeuds multiples avec la même adresse de staking verront une réduction de roll dans le futur. Faire fonctionner plusieurs noeuds avec le même fichier “node_privkey.key” réduira aussi l’intégrité du réseau et sera un point d’attention particulier pour les récompences.

Depuis l’épisode 10, la formule est :

|`Score = 50 * (produced_blocks/selected_slots) * (active_cycles/nb_cycles_episode) * (1 + routable_samples/routability_trials + total_maxim_factor/routability_trials)`

## Enregistrement

Pour valider votre participation au programme de récompense de participation au testnet vous devez vous enregistrer avec votre compte Discord. Vous écrivez quelque chose sur la canal “testnet-rewards-registration” de notre [Discord](https://discord.com/invite/massa) et notre bot (MassaBot) vous enverra les instruction en conversation privée.

## Du score à la récompense

Le lancement du mainnet est plannifié pour 2022.

Les participant pourront réclamer leur récopenses à partir d’un processus d’indentification de type KYC (incluera probablement de la video) pour s’assurer que la même personne ne fait pas plusieurs demandes en accord les lois KYC/AML.

Le score du testnet d’une personne sera la somme des scores de chaque épisode.

La récompense pour le mainnet dépendra du score au testnet. Plus d’informations à ce propos seront données ultérieurement.

Suivant : [Questions fréquentes](URL) / [Frequently Asked Questions](https://docs.massa.net/en/latest/testnet/faq.html)

Précédent : [Accessibilité](./Routability.md) / [Routability](https://docs.massa.net/en/latest/testnet/routability.html)