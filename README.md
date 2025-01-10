# Uniswap : Documentation - Projet BlockChain - Bac 3


# Sommaire

1. [Introduction](#1-introduction)  
2. [Versions de Uniswap](#2-versions-de-uniswap)
3. [ERC-20 et non-ERC-20](#3-erc-20-et-non-erc-20-sur-uniswap)
4. [Cas d'utilisation d'Uniswap](#4-cas-dutilisation-duniswap)  
5. [Pourquoi Uniswap est utile pour la communauté blockchain](#5-pourquoi-uniswap-est-utile-pour-la-communauté-blockchain)  
6. [Comment fonctionne Uniswap](#6-comment-fonctionne-uniswap)
7. [Fonctionnement des Pools de Liquidité](#7-fonctionnement-des-pools-de-liquidité)  
8. [AMM](#8-amm)
9. [Exemple détaillé de l'AMM](#9-exemple-détaillé-de-lamm)
10. [Avantages de l'AMM d'Uniswap](#10-avantages-de-lamm-duniswap)
11. [Slippage](#11-slippage)
12. [Arbitrage et Perte Impermanente](#12-arbitrage-et-perte-impermanente)
13. [Défis et limitations](#13-défis-et-limitations)  
14. [Conclusion](#14-conclusion)  
15. [Références](#15-références) 

---

## 1. Introduction
**Uniswap** est un protocole d'échange décentralisé **(DEX)** construit sur la blockchain **Ethereum**, qui permet aux utilisateurs d'échanger des tokens [ERC-20](#3-erc-20-et-non-erc-20-sur-uniswap), sans avoir besoin d'un intermédiaire centralisé, offrant ainsi une expérience de trading entièrement **décentralisée** dans un [pool de liquidité](#7-fonctionnement-des-pools-de-liquidité) .

**Uniswap** utilise un modèle de [**Market Maker Automatisé (AMM)**](#amm), où la liquidité est fournie par des utilisateurs qui mettent en commun leurs fonds dans des contrats intelligents. Les transactions sont exécutées directement contre ces pools de liquidité au lieu de se baser sur des carnets d'ordres traditionnels.
**Uniswap** bénéficie donc de [2 types d'utilisateurs distincts](#4-cas-dutilisation-duniswap).

---

## 2. Versions de Uniswap

### Uniswap V1
**Date de sortie :** Novembre 2018  
**Caractéristiques principales :**
- **Appariement de token unique :** Chaque pool de liquidité dans Uniswap V1 associe un token [ERC-20](#3-erc-20-et-non-erc-20-sur-uniswap) avec de l'ETH. Par exemple, pour échanger du DAI contre du USDC, un utilisateur doit d'abord échanger du DAI contre de l'ETH, puis de l'ETH contre du USDC.
- **Simplicité :** Une mise en œuvre simple du modèle [AMM](#8-amm) (qui a ses limitations, tel que le [slippage](#11-slippage) et la [perte impermanente](#12-arbitrage-et-perte-impermanente)).
- **Limitations :** 
    - Absence de paires d'échange directes entre [ERC-20](#3-erc-20-et-non-erc-20-sur-uniswap).
    - [Slippage](#11-slippage) (glissement) relativement élevé.


### Uniswap V2
**Date de sortie :** Mai 2020  
**Améliorations par rapport à V1 :**
- **Paires ERC-20 à ERC-20 :** Échange direct entre deux tokens [ERC-20](#3-erc-20-et-non-erc-20-sur-uniswap) sans nécessiter d'ETH comme intermédiaire.
- **Oracles de prix :** Mécanisme de prix moyen pondéré dans le temps, améliorant la fiabilité des flux de prix externes.
- **Flash Swaps :** Emprunt instantané d'actifs depuis les pools de liquidité, à condition qu'ils soient rendus ou payés dans la même transaction.
- **Sécurité améliorée :** Contrats intelligents audités et meilleurs outils pour les développeurs.

### Uniswap V3
**Date de sortie :** Mai 2021  
**Caractéristiques innovantes :**
- **Liquidité concentrée :** Les fournisseurs de liquidité (LPs) peuvent spécifier des plages de prix pour leurs fonds, augmentant ainsi l'efficacité du capital et réduisant le glissement.
- **Paliers de frais multiples :** Différentes structures de frais (par exemple, 0,05 %, 0,3 %, 1 %) adaptées à divers couples de tokens, et compatible avec les **Layer 2** (ex. Optimism, Arbitrum), ce qui réduit les frais de gaz, et optimise les frais pour le trading de stablecoins et les paires volatiles.
- **Positions de liquidité non fongibles :** Les positions des LP sont représentées sous forme de NFT (non fungible tokens), permettant des stratégies uniques et personnalisables.
- **Flexibilité :** Prise en charge des ajustements dynamiques des frais et des mécanismes de gouvernance améliorés.

### Uniswap V4
**Pas encore officiellement disponbile.**

**Améliorations attendues :**
La V4 pourrait introduire des fonctionnalités encore plus avancées, telles que :  
- **Liquidité dynamique :** Adaptation automatique des plages de liquidité en fonction des conditions du marché.  
- **Optimisation inter-chaînes :** Meilleure intégration avec des solutions multi-chaînes et L2 (Layer 2).  
- **Réduction des frais de gas :** Utilisation accrue de solutions de roll-up pour minimiser les coûts.  
- **Gouvernance améliorée :** Mécanismes de vote et de participation simplifiés pour les détenteurs de tokens UNI.  

---

## 3. ERC-20 et non-ERC-20 sur Uniswap

### ERC-20
Les **Tokens ERC-20** sont un standard de jeton sur la blockchain **Ethereum**, défini par une série de règles que les smart contracts doivent suivre.
Les jetons ERC-20 sont interchangeables et suivent des fonctions standards : `totalSupply`, `balanceOf`, `transfer`, `approve`, `transferFrom`.

**Par exemple, voici quelques tokens ERC-20 :**
- **USDT (Tether)**  
- **USDC (USD Coin)**  
- **DAI (Dai Stablecoin)**  
- **LINK (Chainlink)** 
- **UNI (Token de Uniswap)** 

Tous les tokens ERC-20 sont créés et gérés par des **smart contracts** sur la blockchain **Ethereum**.  
Les transactions de tokens ERC-20 nécessitent des frais de gaz payés en **ETH**.

### Tokens Non-ERC-20
Les tokens **non-ERC-20** sont des jetons qui ne suivent pas ce standard.  
Ils peuvent appartenir à d'autres blockchains ou utiliser d'autres standards comme :  
  - **ERC-721** : Tokens non fongibles (NFTs)  
  - **ERC-1155** : Tokens multi-standards (NFTs + tokens fongibles)  
  - **BEP-20** : Tokens sur la Binance Smart Chain  
  - **TRC-20** : Tokens sur Tron  

Quelques exemples :
- **CryptoKitties (ERC-721)**  
- **Wrapped Bitcoin (WBTC)** suit ERC-20, mais le **Bitcoin natif (BTC)** ne suit aucun standard Ethereum.  
- **BNB** sur Binance Smart Chain utilise **BEP-20**.  

Ceci dit:
- Les tokens non-ERC-20 ne sont pas directement compatibles avec Ethereum sauf s’ils sont "wrapés" (par ex. **WBTC**).  
- Les bridges sont nécessaires pour interagir avec des blockchains différentes.  (Des protocoles, ou smart contracts qui facilitent le transfert d’actifs entre des blockchains indépendantes)

---

## 4. Cas d'utilisation d'Uniswap

1. Quand on désire un trading fluide, et **sans permission**, de tokens ERC-20.
2. Si on veut contribuer aux pools de liquidité et gagner des frais de change (jusqu'à 0.3%) sur les trades éventuels.
3. Facilite la découverte des prix de manière transparente et en temps réel pour les tokens.

#### Deux types d'utilisateurs distincts :

1. **Traders :**
   - **Description :** Les traders utilisent Uniswap pour échanger des tokens de manière rapide, et sans permission. Ils bénéficient d'une accessibilité constante, même pour des tokens peu liquides ou nouveaux (attention aux tokens scam!).
   - **Avantages pour les traders :**
     - Pas besoin de créer de compte ni de fournir des informations personnelles.
     - Ils peuvent direcement échanger depuis leur portefeuille Ethereum.
     - Ils ont accès à une large gamme de tokens, y compris ceux qui ne sont pas encore disponibles sur des plateformes centralisées.
     - Ils bénéficient d'une résilience face aux restrictions des bourses centralisées.
     - Découverte de prix efficace grâce aux pools de liquidité.
   - **Cas d'utilisation spécifiques :**
     - [Arbitrage](#12-arbitrage-et-perte-impermanente) : Ils peuvent profiter des différences de prix entre Uniswap et d'autres plateformes.
     - Trading de niche : Investir dans des tokens émergents ou à faible capitalisation.
2. **Fournisseurs de liquidité (LPs) :**
   - **Description :** Les fournisseurs de liquidité déposent des paires de tokens (par exemple, ETH et USDT) dans les pools de liquidité d’Uniswap. En retour, ils gagnent une part des frais générés par les transactions réalisées via le pool.
   - **Avantages pour les LPs :**
     - Générer des revenus passifs grâce aux frais de trading (généralement 0,3 % par transaction).
     - Possibilité de concentrer leur liquidité (V3) dans des plages de prix spécifiques pour maximiser les rendements.
     - Diversification des actifs en profitant de leurs paires sur Uniswap.
   - **Risques à considérer :**
     - **Pertes impermanentes :** Une baisse significative du prix d'un token peut entraîner des pertes potentielles.
     - **Volatilité du marché :** Les pools exposés à des actifs volatils peuvent être sujets à des fluctuations de valeur.
     [Plus de détails sur les risques des investisseurs ici](#11-slippage)

---
***Illustration des utilisateurs UNISWAP :***

![Alt text](https://docs.uniswap.org/assets/images/anatomy-d22fb7ab46013a1195f086ee672468c7.jpg)
[Source: How Uniswap works](https://docs.uniswap.org/contracts/v2/concepts/protocol-overview/how-uniswap-works)

---

## 5. Pourquoi Uniswap est utile pour la communauté blockchain
Uniswap est devenu important dans l'écosystème ETC, qui permet :

1. L'élimination des intermédiaires, ce qui garantit une plus grande transparence et confiance.
2. À tout le monde d'échanger des tokens ou créer des pools de liquidité sans aucune restriction ou permission.
3. De servir de plateforme pour tester et lancer de nouveaux tokens et projets DeFi.
4. Une gouvernance décentralisée, avec des décisions prises collectivement par les détenteurs de tokens UNI.

---

## 6. Comment fonctionne Uniswap


1. **Modèle de Market Maker Automatisé :**
   - Uniswap remplace les carnets d'ordres traditionnels par des pools de liquidité, permettant aux échanges de se produire **sans correspondance directe entre acheteurs et vendeurs.**
   - Les prix sont déterminés **algorithmiquement** en utilisant la formule du produit constant : \( x \times y = k \), où \( x \) et \( y \) sont les réserves du pool, et \( k \) est une constante. 
   [Plus détaillé dans le point prochain.](#8-amm)  

2. **Efficacité du capital (V3) :**
   - La liquidité concentrée permet aux LPs d'allouer leurs fonds plus efficacement, maximisant les rendements tout en réduisant les pertes impermanentes.

3. **Architecture composable :**
   - S'intègre parfaitement à d'autres protocoles DeFi, permettant des opérations financières complexes telles que l'arbitrage, le prêt et la création d'actifs synthétiques.

4. **Modèle de frais transparent :**
   - Les frais sont entièrement distribués aux LPs, favorisant un écosystème inclusif.

---
## 7. Fonctionnement des Pools de Liquidité

### Dépôts de liquidité
Les **investisseurs** (appelés **fournisseurs de liquidité**) déposent deux actifs dans un ratio équivalent (en valeur) dans un pool. En retour, ils reçoivent des **jetons LP (Liquidity Provider)** qui représentent leur part du pool.

### Transactions
Lorsqu'un **trader** échange un actif contre un autre via Uniswap, il interagit avec le pool de liquidité :
- L'actif donné est ajouté au pool.
- L'actif reçu est retiré du pool.
- Le prix est ajusté automatiquement en fonction de la formule \( x \times y = k \) utilisant la technologie [AMM](#8-amm).

### Incitations pour les fournisseurs de liquidité
Les fournisseurs de liquidité gagnent une part des frais de transaction (généralement 0,3%) collectés sur chaque échange.

---
## 8. AMM

Un AMM est un protocole décentralisé qui permet de négocier des crypto-monnaies directement depuis des contrats intelligents (smart contracts). Contrairement aux bourses traditionnelles qui utilisent un carnet d'ordres (order book), un AMM fonctionne grâce à des **pools de liquidités** (liquidity pools).

### Points clés :
- **Absence de carnet d'ordres** : Les transactions sont exécutées en fonction d'algorithmes préprogrammés.
- **Pools de liquidité** : Les utilisateurs fournissent des liquidités en déposant des paires d'actifs (par exemple, ETH et USDT) dans un pool.
- **Échanges automatisés** : Les prix des actifs sont déterminés par une formule mathématique.

#### La Formule Mathématique : \( x \times y = k \)

Uniswap utilise une formule simple, mais puissante, pour déterminer les prix des actifs dans un pool :

\[
x \times y = k
\]

#### Explication :
- **\( x \)** : Quantité de l'actif A (par exemple, ETH) dans le pool.
- **\( y \)** : Quantité de l'actif B (par exemple, USDT) dans le pool.
- **\( k \)** : Une constante qui reste inchangée (sauf en cas de modification du pool).

Cette formule garantit que le produit des quantités des deux actifs reste constant après chaque transaction. Cela signifie qu'au fur et à mesure qu'un actif est acheté, son prix augmente en raison de la diminution de sa disponibilité relative. Exemple détaillé dans le prochain chapitre.

---

## 9. Exemple détaillé de l'AMM

Supposons qu’un fournisseur de liquidité crée un pool contenant :

- 100 pommes (`x = 100`),
- 200 pommes de terre (`y = 200`).

La constante est donc calculée comme suit :  
`k = x * y = 100 * 200 = 20 000`.

#### Échange d'une pomme contre des pommes de terre
Un utilisateur veut échanger **1 pomme** contre des pommes de terre.  
Le système AMM utilise la formule pour maintenir la constante `k`.

#### Avant l'échange :
- `x = 100` (pommes dans le pool),
- `y = 200` (pommes de terre dans le pool),
- `k = 20 000`.

L'utilisateur ajoute 1 pomme au pool, ce qui fait passer la quantité de pommes à `x = 101`.  
Pour maintenir `k = 20 000`, le système doit ajuster `y` :

`x' * y' = k 101 * y' = 20 000 y' = 20 000 / 101 ≈ 198.02`

Ainsi, après l'échange :
- Le pool contient désormais `x = 101` pommes,
- Le pool contient `y = 198.02` pommes de terre.

L'utilisateur reçoit donc **200 - 198.02 = 1.98 pommes de terre** en échange de sa pomme.

#### Impact :
Cet échange illustre la courbe de prix non linéaire propre aux AMM : plus l'utilisateur ajoute ou retire des actifs, plus l'impact sur le prix est grand (slippage).

#### Échange de pommes de terre contre des pommes
Un autre utilisateur veut maintenant échanger **2 pommes de terre** contre des pommes.

#### Avant l'échange :
- `x = 101`,
- `y = 198.02`.

L'utilisateur retire 2 pommes de terre du pool, ce qui fait passer `y` à `196.02`.  
Pour maintenir `k = 20 000`, le système ajuste `x` :

`x' * y' = k x' * 196.02 = 20 000 x' = 20 000 / 196.02 ≈ 102.04`


L'utilisateur ajoute donc **102.04 - 101 = 1.04 pommes** au pool pour récupérer ses 2 pommes de terre.

##### La formule clé : `x * y = k` assure un équilibre constant, mais introduit également une dépendance des prix basée sur les quantités relatives des actifs.

#### Visualisation de la courbe de prix
La relation entre les deux actifs peut être représentée par une hyperbole décrivant la formule `x * y = k`. Par exemple :

- Lorsque l'un des actifs diminue fortement, l'autre augmente en prix de manière exponentielle.
- Cela garantit qu'un actif ne peut jamais être totalement épuisé, sauf si quelqu'un paie une valeur infinie pour le retirer.

Ce mécanisme est au cœur de la technologie des AMM, comme celle utilisée dans **Uniswap**.

---

## 10. Avantages de l'AMM d'Uniswap

1. **Décentralisation totale** : Les utilisateurs gardent le contrôle de leurs fonds.
2. **Liquidité continue** : Pas besoin de contreparties actives pour exécuter des ordres.
3. **Simplicité et automatisation** : Les prix sont calculés automatiquement, et en temps réel.

Par contre, l'AMM a des gros défauts, [Slippage](#11-slippage) et [Perte Impermanente](#12-arbitrage-et-perte-impermanente).

---

## 11. Slippage

Le **slippage** (glissement) représente la différence entre le prix attendu d'un trade et le prix réel auquel il est exécuté.  
Cela se produit lorsque la liquidité du marché est insuffisante ou lorsqu'une grande commande affecte le prix d'un actif.

**Causes du Slippage :**
- **Volatilité des marchés** : Des mouvements de prix rapides peuvent provoquer des écarts.  
- **Faible liquidité** : Des pools de liquidité peu profonds entraînent des écarts de prix importants.  
- **Transactions importantes** : De gros ordres peuvent modifier le prix dans les [AMM](#8-amm).

**Solution :**
- On peut définir une **tolérance de slippage** dans les plateformes DeFi (ex. : 0,5 %).  
- Il vaut mieux utiliser des pools de liquidité profonds pour éviter la volatilité.

---

## 12. Arbitrage et Perte Impermanente

L’**arbitrage** consiste à profiter des différences de prix d’un même actif en le comparant à plusieurs plateformes, dans le but de réaliser un profit.  
Dans le contexte des **AMM** (Automated Market Makers) comme **Uniswap**, l’arbitrage peut provoquer des glissements pour les investisseurs.

Quand le prix d’un actif est différent entre **Uniswap** et une autre plateforme (**Binance**, **Coinbase**), les arbitragistes achètent l’actif là où il est moins cher et le revendent là où il est plus cher.  

**Exemple :**  
- Sur **Uniswap**, 1 **ETH** = **2 000 USDC**.  
- Sur **Binance**, 1 **ETH** = **2 050 USDC**.  
- Un arbitragiste achète de l’**ETH** sur **Uniswap** et le revend sur **Binance**, réalisant un profit de **50 USDC** par ETH.

**Impact sur les Fournisseurs de Liquidité (LP) :**
- Les arbitrages fréquents rééquilibrent les prix mais exposent les LP à la **perte impermanente**.  
- Le réajustement des ratios d’actifs dans les pools entraîne une redistribution des actifs, souvent au détriment des LP.

La **perte impermanente** survient lorsqu’un LP fournit des actifs dans un pool de liquidité et que les prix relatifs des actifs fluctuent.  
Cette perte est dite *impermanente* car elle ne devient réelle que si le LP retire ses fonds après une variation de prix.

- Les arbitragistes profitent des déséquilibres de prix, forçant le pool à rééquilibrer ses actifs.  
- Ce rééquilibrage fait que les LP détiennent plus d’un actif qui a perdu de la valeur et moins de celui qui a gagné en valeur.  
- **Résultat :** Le LP aurait été plus rentable en conservant ses actifs sans les déposer dans le pool.

#### Exemple  
1. Un LP dépose 1 **ETH** (**2 000 USDC**) et **2 000 USDC** dans un pool.  
2. Le prix de l’**ETH** grimpe à **3 000 USDC**.  
3. Les arbitragistes achètent de l’**ETH** dans le pool pour le revendre ailleurs, diminuant la quantité d’**ETH** détenue par le LP.  
4. Lors du retrait, le LP récupère moins d’**ETH** et plus d’**USDC**, entraînant une **perte impermanente**.

---

## 13. Défis et limitations

Bien qu'Uniswap ait révolutionné le trading décentralisé, il présente certains défis :

1. **Coûts élevés de gas :**
   - Les transactions sur Ethereum peuvent être coûteuses en période de congestion réseau.

2. **Pertes impermanentes :**
   - Les fournisseurs de liquidité peuvent subir des pertes par rapport à la simple détention de leurs actifs en raison de la volatilité des prix.

3. **Concurrence :**
   - D'autres AMM et DEX émergent, certains offrant des frais inférieurs ou des fonctionnalités uniques.

4. **Glissement (Slippage) :** 
   - Dans les pools à faible liquidité, les grandes transactions peuvent provoquer des variations de prix significatives.

5. **Risques liés aux scams :**
   - **Fake Tokens** :
     Les escrocs créent des tokens imitant des projets réputés ou populaires, mais sans aucune valeur réelle.

   - **Rug Pulls** :
     Les développeurs retirent soudainement toute la liquidité après avoir attiré des investisseurs.

   - **Smart Contracts Malveillants** :
     Certains tokens sont associés à des contrats intelligents qui bloquent ou limitent la revente.

   - **Hyperinflation Programmée** :
     Les escrocs programment des tokens pour générer massivement de nouvelles unités après l'investissement initial.

   - **Manque de liquidité crédible** :
     Les pools de liquidité avec de faibles montants ou des liquidités récemment ajoutées sans garantie peuvent indiquer des risques de volatilité et de slippage.

#### Précautions :
- Vérifiez l'adresse du contrat sur une source officielle.
- Recherchez des informations fiables sur le projet et ses créateurs.
- Utilisez des outils comme Token Sniffer ou DEXTools pour analyser les tokens.
- Méfiez-vous des tokens récemment listés ou avec une communauté suspecte.

---

## 14. Conclusion
Uniswap a transformé l'écosystème blockchain en pionnier du trading décentralisé et sans permission. Son évolution à travers les versions V1, V2 et V3 démontre un engagement constant envers l'innovation, l'évolutivité et l'autonomisation des utilisateurs.

En supprimant les barrières traditionnelles et en favorisant un écosystème DeFi dynamique, Uniswap a établi la norme pour les échanges décentralisés, consolidant son rôle de protocole fondamental dans la communauté blockchain.

---

## 15. Références
- [Site officiel de Uniswap](https://uniswap.org)
- [Documentation Uniswap](https://docs.uniswap.org)
- [Fondation Ethereum](https://ethereum.org)
- [DeFi Pulse - Uniswap](https://defipulse.com/uniswap)
- [How Uniswap is changing the DEX space](https://liquidityfinder.com/insight/crypto/how-uniswap-is-changing-the-dex-space)
- [How Uniswap works](https://docs.uniswap.org/contracts/v2/concepts/protocol-overview/how-uniswap-works)
- [What is ERC-20?](https://www.coinbase.com/learn/crypto-glossary/what-is-erc-20)

##### Vidéos YouTube interessantes et bien expliquées :
- [What is Uniswap - A Beginner's Guide (2024 Updated)](https://www.youtube.com/watch?v=dIneNZTnFMw)
- [What is Uniswap? (Animated) Decentralized Exchange + UNI Token](https://www.youtube.com/watch?v=DLu35sIqVTM&t=306s)
- [What is an Automated Market Maker? (Liquidity Pool Algorithm)](https://www.youtube.com/watch?v=1PbZMudPP5E)
- [What is a Liquidity Pool in Crypto? (Animated)](https://www.youtube.com/watch?v=dVJzcFDo498&t=19s)
- [What is DeFi? (Decentralized Finance Animated)](https://www.youtube.com/watch?v=17QRFlml4pA&t=618s)

---

### Auteurs:
Elie Kheirallah
John Yazbeck
Alain Nitunga Michel

Fait le `07-01-2025`
