# MODULE 06 — BLOCS FC, FB ET DB
## Leçon 4 : Les blocs FB (Function Block)

### 1. Définition

Un FB (Function Block) est un bloc de programmation réutilisable qui possède une mémoire associée appelée DB d'instance.

Il permet de gérer des équipements industriels dont le fonctionnement nécessite la mémorisation d'informations entre plusieurs cycles automate.

Exemples :
- Gestion d'une pompe.
- Commande d'un moteur.
- Gestion d'un ventilateur.
- Pilotage d'un convoyeur.
- Gestion d'une séquence industrielle.

### 2. Différence entre FC et FB

| Critère | FC | FB |
|---|---|---|
| Signification | Function | Function Block |
| DB d'instance | Non requis | Requis |
| Mémoire STAT propre | Non | Oui |
| Variables TEMP | Oui | Oui |
| Réutilisation | Oui | Oui |
| Application typique | Calculs et conversions | Gestion d'équipements |

Un FC peut accéder à des DB globaux. L'absence de DB d'instance ne signifie donc pas qu'il ne peut pas utiliser de données mémorisées ailleurs.

### 3. Les variables STAT et TEMP

**STAT :**
- Variables statiques du FB.
- Valeurs conservées entre les appels du bloc.
- Stockées dans le DB d'instance.
- Leur conservation après une coupure dépend notamment de la configuration de rémanence.

**TEMP :**
- Variables temporaires.
- Utilisées pour les calculs intermédiaires.
- Ne garantissent pas la conservation de leur valeur entre les appels.
- Doivent être affectées avant leur utilisation.

### 4. Réutilisation d'un FB

Un même FB peut gérer plusieurs équipements.

Exemple :

| Équipement | Bloc utilisé | DB d'instance |
|---|---|---|
| Pompe 1 | FB_GESTION_POMPE | DB_POMPE_1 |
| Pompe 2 | FB_GESTION_POMPE | DB_POMPE_2 |
| Pompe 3 | FB_GESTION_POMPE | DB_POMPE_3 |

Chaque instance possède ses propres données STAT.

Ainsi, les trois pompes utilisent la même logique, mais conservent des états et des compteurs indépendants.

### 5. Application pratique : FB_GESTION_POMPE

Le FB étudié comporte les variables suivantes :

| Section | Variable | Type | Fonction |
|---|---|---|---|
| Input | Cmd_Marche | BOOL | Demande de marche |
| Input | Cmd_Arret | BOOL | Demande d'arrêt |
| Input | Mode_Auto | BOOL | Autorisation du mode automatique |
| Input | Defaut_Moteur | BOOL | Défaut moteur |
| Output | Pompe_Marche | BOOL | État de marche de la pompe |
| Static | Etat_Memorise | BOOL | État mémorisé |
| Static | Nb_Demarrages | INT | Nombre de démarrages |
| Temp | Front_Detecte | BOOL | Impulsion de détection |
| Temp | Demarrage_Autorise | BOOL | Condition d'autorisation |

### 6. Mémorisation de l'état de la pompe

Une bascule SR permet de mémoriser l'état de fonctionnement.

- S : demande de mise en marche.
- R1 : commande d'arrêt ou défaut.
- Q : état mémorisé.

Dans la convention Siemens :
- SR : Reset dominant.
- RS : Set dominant.

La sortie mémorisée est ensuite affectée à `Pompe_Marche`.

### 7. Détection des fronts avec R_TRIG

R_TRIG signifie Rising Edge Trigger.

Il détecte le passage d'un signal de FALSE à TRUE.

| Entrée CLK | Sortie Q |
|---|---|
| FALSE maintenu | FALSE |
| Passage FALSE → TRUE | TRUE pendant un cycle |
| TRUE maintenu | FALSE après l'impulsion |

Un appui maintenu pendant plusieurs secondes ne produit donc qu'une seule impulsion.

### 8. Autorisation de démarrage

La condition logique étudiée est :

`Demarrage_Autorise = Mode_Auto AND Cmd_Marche AND NOT Cmd_Arret AND NOT Defaut_Moteur`

Le démarrage est autorisé uniquement lorsque :
- Le mode automatique est actif.
- Une demande de marche est présente.
- Aucune commande d'arrêt n'est active.
- Aucun défaut moteur n'est présent.

Cette condition est utilisée à l'entrée CLK de R_TRIG afin de détecter le front montant de l'autorisation.

### 9. Comptage des démarrages

Le bloc ADD réalise l'incrémentation :

`Nb_Demarrages := Nb_Demarrages + 1`

Le compteur est de type STAT afin de conserver sa valeur entre les appels du FB.

Le front montant détecté par R_TRIG déclenche l'incrémentation.

**Remarque technique :** dans une application industrielle complète, le comptage doit être associé au démarrage effectivement réalisé, idéalement à la confirmation de marche du contacteur ou du retour d'état moteur. Le comptage de l'autorisation seule ne prouve pas que le moteur a réellement démarré.

### 10. Contrôle de compréhension

1. Quelle est la différence fondamentale entre FC et FB ?
2. Où sont stockées les variables STAT d'un FB ?
3. Pourquoi utilise-t-on R_TRIG pour détecter un démarrage ?
4. Combien d'impulsions R_TRIG produit-il si son entrée reste à TRUE pendant 30 secondes ?
5. Quel est le rôle du DB d'instance ?
6. Pourquoi faut-il bloquer le démarrage en cas de défaut moteur ?

### 11. Travaux pratiques réalisés

- Création et configuration de `FB_GESTION_POMPE`.
- Configuration des variables Input, Output, Static et Temp.
- Mise en place de la mémorisation de l'état de la pompe.
- Insertion de R_TRIG.
- Création de la condition de démarrage autorisé.
- Mise en place du comptage des démarrages.
- Compilation du bloc.

### 12. État de validation

| Vérification | Résultat |
|---|---|
| Configuration du FB | Réalisée |
| Logique de mémorisation | Réalisée |
| Détection de front | Réalisée |
| Comptage des démarrages | Réalisé |
| Compilation | Réussie |
| Simulation PLCSIM | À réaliser |

**Conclusion :**

Les blocs FB sont particulièrement adaptés à la programmation des équipements industriels nécessitant une mémoire propre. Leur association avec des DB d'instance permet de réutiliser une même logique sur plusieurs équipements tout en conservant des données indépendantes.
