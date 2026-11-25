# MODULE 06 — BLOCS FC, FB ET DB
## Leçon 1 : Architecture des blocs de programmation

### 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Expliquer le rôle des différents blocs d'un programme Siemens.
- Distinguer les OB, FC, FB et DB.
- Comprendre l'intérêt de la programmation modulaire.
- Expliquer la différence entre mémoire statique et mémoire temporaire.
- Comprendre les DB d'instance et les DB globaux.
- Organiser un programme autour de plusieurs équipements industriels.

---

### 2. Pourquoi structurer un programme automate ?

Dans une installation industrielle, un automate peut gérer :

- Plusieurs moteurs.
- Des pompes.
- Des capteurs.
- Des vannes.
- Des alarmes.
- Des temporisations.
- Des mesures analogiques.

Si toute la programmation est réalisée directement dans OB1, le programme peut devenir très long et difficile à maintenir.

La programmation modulaire consiste à diviser le programme en plusieurs blocs spécialisés.

Exemple :

```text
                  OB1
                   |
       +-----------+-----------+
       |           |           |
       v           v           v
   FC_POMPE     FC_ALARMES   FC_CONVERSION
       |
       v
   FB_GESTION_POMPE
       |
       +-----------+
       |           |
       v           v
   DB_POMPE_1  DB_POMPE_2
```

Avantages :

- Programme plus lisible.
- Maintenance facilitée.
- Réutilisation du code.
- Diagnostic simplifié.
- Organisation par équipement.

---

### 3. Les quatre grandes familles de blocs

#### 3.1. OB — Organization Block

Les OB sont les blocs d'organisation.

Ils constituent les points d'entrée du programme automate et organisent son exécution.

OB1 est généralement le bloc principal exécuté cycliquement.

Cycle simplifié :

```text
Lecture des entrées
        |
        v
Exécution de OB1
        |
        v
Appel des FC et FB
        |
        v
Traitement des données
        |
        v
Mise à jour des sorties
        |
        v
Nouveau cycle
```

Autres familles d'OB :

- OB de démarrage.
- OB d'interruption cyclique.
- OB d'interruption matérielle.
- OB de diagnostic et d'erreur.

La disponibilité et le comportement des OB dépendent du modèle d'automate et de sa configuration.

Important : un bloc créé dans le projet ne s'exécute pas automatiquement. Il doit être appelé par un bloc exécuté ou déclenché par un mécanisme système approprié.

#### 3.2. FC — Function

Un FC est un bloc fonctionnel sans mémoire d'instance propre.

Il permet de réaliser un traitement spécifique.

Exemples :

- Conversion analogique.
- Calcul d'une moyenne.
- Comparaison de valeurs.
- Calcul d'un débit.
- Traitement d'une condition.

Un FC peut recevoir des paramètres, traiter des données et retourner un résultat.

Il peut également accéder à des données globales ou à d'autres données externes.

Exemple de conversion :

```text
Niveau (%) = (Valeur brute / 27648) × 100
```

Pour une valeur brute de 13824 :

```text
Niveau = (13824 / 27648) × 100
Niveau = 50 %
```

Les catégories de variables d'un FC :

| Catégorie | Fonction |
|---|---|
| Input | Paramètres d'entrée |
| Output | Paramètres de sortie |
| InOut | Paramètres lus et modifiés |
| Temp | Variables temporaires |
| Return | Valeur de retour éventuelle |

Un FC n'a pas de DB d'instance. Ses variables TEMP ne doivent pas être considérées comme des données conservées entre deux appels.

#### 3.3. FB — Function Block

Un FB est un bloc fonctionnel qui possède une mémoire associée : le DB d'instance.

Il est particulièrement adapté aux équipements industriels qui doivent conserver leur état.

Exemples :

- Gestion d'une pompe.
- Commande d'un moteur.
- Gestion d'une vanne.
- Contrôle d'un convoyeur.

Un même FB peut être réutilisé pour plusieurs équipements.

Exemple :

```text
              FB_GESTION_POMPE
                     |
          +----------+----------+
          |                     |
          v                     v
      DB_POMPE_1            DB_POMPE_2
          |                     |
          v                     v
       Pompe 1               Pompe 2
```

Chaque instance possède ses propres données.

#### 3.4. DB — Data Block

Un DB est un bloc de données.

Il sert à stocker les informations utilisées par le programme.

Il existe principalement deux catégories :

**DB d'instance :**

- Associé à un FB.
- Contient les données statiques de son instance.
- Permet de conserver indépendamment les états de plusieurs équipements.

**DB global :**

- Non associé à un FB particulier.
- Centralise les données accessibles depuis plusieurs blocs, selon les règles d'accès.
- Peut contenir des paramètres communs, des mesures et des consignes.

---

### 4. Différence entre TEMP et STAT

| Critère | TEMP | STAT |
|---|---|---|
| Signification | Temporaire | Statique |
| Conservation entre appels | Non garantie | Oui |
| Stockage | Mémoire temporaire du bloc | DB d'instance pour un FB |
| Utilisation | Calcul intermédiaire | État mémorisé |

Exemple :

Dans un FB de gestion de pompe :

- `Etat_Memorise` : STAT.
- `Nb_Demarrages` : STAT.
- `Calcul_Intermediaire` : TEMP.

Les variables STAT conservent leurs valeurs entre les appels du FB, dans la mesure où les données ne sont pas réinitialisées ou modifiées.

Les variables TEMP doivent être affectées avant utilisation.

---

### 5. Différence entre FC et FB

| Critère | FC | FB |
|---|---|---|
| Nom complet | Function | Function Block |
| DB d'instance | Non | Oui |
| Mémoire STAT propre | Non | Oui |
| Paramètres d'entrée | Oui | Oui |
| Paramètres de sortie | Oui | Oui |
| Réutilisation | Oui | Oui |
| Équipements avec états indépendants | Moins adapté | Très adapté |

Exemple :

Pour convertir dix valeurs analogiques, un FC peut être appelé plusieurs fois.

Pour gérer dix moteurs avec des états indépendants, un FB avec dix instances est une solution adaptée.

---

### 6. Les priorités des instructions SR et RS dans TIA Portal

Attention à la convention Siemens :

- SR possède les entrées S et R1 : RESET dominant.
- RS possède les entrées R et S1 : SET dominant.

Lorsque les deux entrées sont à TRUE :

| Instruction | Résultat |
|---|---|
| SR | Q = FALSE |
| RS | Q = TRUE |

La règle pratique est que l'entrée dont le nom se termine par 1 est dominante.

Pour une pompe, lorsque l'arrêt ou un défaut doit être prioritaire sur la marche, l'instruction SR peut être utilisée.

---

### 7. Application pratique réalisée dans TIA Portal V17

Configuration utilisée :

- Automate : S7-1200 CPU 1214C DC/DC/DC.
- Environnement : TIA Portal V17.
- Langages utilisés : SCL et LAD.

#### 7.1. Création de FC_CONVERSION

Bloc créé : `FC_CONVERSION`.

Déclarations :

| Nom | Type | Catégorie |
|---|---|---|
| Valeur_Brute | INT | Input |
| Niveau_Pourcent | REAL | Output |
| Calcul | REAL | Temp |

Code SCL :

```scl
Calcul := INT_TO_REAL(Valeur_Brute) / 27648.0 * 100.0;
Niveau_Pourcent := Calcul;
```

Ce bloc convertit une valeur analogique brute en pourcentage.

#### 7.2. Création de FB_GESTION_POMPE

Bloc créé : `FB_GESTION_POMPE`.

Déclarations :

| Nom | Type | Catégorie |
|---|---|---|
| Cmd_Marche | BOOL | Input |
| Cmd_Arret | BOOL | Input |
| Mode_Auto | BOOL | Input |
| Defaut_Moteur | BOOL | Input |
| Pompe_Marche | BOOL | Output |
| Etat_Memorise | BOOL | Static |
| Nb_Demarrages | INT | Static |

Le programme utilise une mémoire de type SR pour mémoriser l'état de fonctionnement, avec priorité au RESET.

Réseau de sortie :

```text
|----[ Etat_Memorise ]----( Pompe_Marche )----|
```

Remarque : les variables déclarées ne sont pas toutes encore exploitées dans la logique. Leur utilisation sera approfondie dans les prochaines leçons.

#### 7.3. Création des DB d'instance

Deux instances ont été créées :

- `DB_POMPE_1`
- `DB_POMPE_2`

Elles sont toutes deux associées à `FB_GESTION_POMPE`.

Elles permettent de gérer deux pompes avec des données indépendantes.

#### 7.4. Création du DB global

Bloc créé : `DB_PARAMETRES`.

Variables :

| Nom | Type | Valeur initiale |
|---|---|---|
| Pression_Min | REAL | 2.5 |
| Pression_Max | REAL | 8.0 |
| Temporisation_Generale | TIME | T#5S |
| Nb_Max_Demarrages | INT | 10 |

Ce bloc centralise des paramètres communs à l'installation.

#### 7.5. Compilation

Une compilation complète du logiciel a été réalisée.

Résultat confirmé : compilation réussie.

---

### 8. Exercices et corrections

#### Exercice 1 — Choix des blocs

1. Bloc principal d'organisation : OB1.
2. Conversion analogique : FC.
3. Gestion de six moteurs avec états indépendants : un FB avec six DB d'instance.
4. Paramètres communs : DB global.

#### Exercice 2 — Compréhension des mémoires

1. Les données STAT de FB_P1 sont conservées dans DB_P1.
2. L'appel de FB_P2 ne modifie pas automatiquement les données de DB_P1.
3. Une variable de comptage persistante est déclarée STAT.
4. Un FC ne possède pas de DB d'instance.

---

### 9. QCM — Réponses validées

| Question | Bonne réponse | Explication |
|---|---|---|
| Avantage de la modularisation | B | Facilite l'organisation |
| FC non appelé | C | Il ne s'exécute pas |
| Rôle d'OB1 | B | Organise l'exécution |
| FC avec résultat | C | Peut retourner un résultat |
| Six moteurs indépendants | C | FB avec instances |
| Deux pompes | B | FB et DB d'instance |

Les six questions ont été correctement répondues.

---

### 10. À retenir absolument

1. OB organise l'exécution du programme.
2. OB1 est généralement le bloc cyclique principal.
3. FC réalise un traitement sans DB d'instance.
4. FB possède une mémoire d'instance.
5. DB d'instance conserve les données propres à une instance de FB.
6. DB global centralise des données communes.
7. STAT conserve les valeurs entre les appels du FB.
8. TEMP ne garantit pas la conservation des valeurs entre les appels.
9. Un bloc ne s'exécute pas simplement parce qu'il existe.
10. La modularisation facilite la maintenance et l'évolution des programmes industriels.

---

### 11. Conclusion de la leçon

Cette première leçon nous a permis de comprendre l'architecture modulaire d'un programme Siemens.

Nous avons créé un FC de conversion analogique, un FB de gestion de pompe, deux DB d'instance et un DB global.

La compilation a été réussie.

La prochaine leçon approfondira les blocs FC : création, paramètres, variables, programmation, appels et applications industrielles.
