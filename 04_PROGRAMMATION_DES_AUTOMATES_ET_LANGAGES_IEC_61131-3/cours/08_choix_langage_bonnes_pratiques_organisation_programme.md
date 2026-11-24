# MODULE 04 — PROGRAMMATION DES AUTOMATES ET LANGAGES IEC 61131-3

# LEÇON 8 — CHOIX DU LANGAGE ET BONNES PRATIQUES D'ORGANISATION D'UN PROGRAMME AUTOMATE

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Choisir un langage de programmation adapté à une fonction industrielle.
- Distinguer les domaines d'utilisation de LAD, FBD, SCL et GRAPH.
- Comprendre comment plusieurs langages peuvent coexister dans un même projet TIA Portal.
- Organiser un programme automate en blocs spécialisés.
- Distinguer les rôles des blocs OB, FC, FB et DB.
- Appliquer des conventions de nommage aux variables.
- Documenter correctement un programme pour faciliter sa maintenance.

---

## 2. Critères de choix d'un langage de programmation

Un automate programmable industriel peut être programmé avec différents langages. Le choix dépend principalement de la fonction à réaliser.

Les principaux critères sont :

| Critère | Explication |
|---|---|
| Nature du traitement | Commande logique, calcul, régulation ou séquence. |
| Complexité | Nombre de conditions, de calculs et de traitements. |
| Lisibilité | Facilité de compréhension du programme. |
| Maintenance | Facilité de diagnostic et de modification. |
| Compétences de l'équipe | Langages maîtrisés par les techniciens et automaticiens. |
| Évolution | Possibilité d'ajouter de nouvelles fonctions. |

### Exemple industriel

Nous devons commander un moteur triphasé avec :

- Un bouton Marche.
- Un bouton Arrêt.
- Une autorisation de fonctionnement.
- Un contact de défaut thermique.
- Une sortie de commande du contacteur.

Le langage LAD est particulièrement adapté, car la commande repose essentiellement sur des conditions logiques représentées par des contacts et des bobines.

---

## 3. Comparaison des principaux langages

### 3.1. LAD — Ladder Diagram

LAD est adapté aux commandes logiques.

Applications :
- Commande de moteurs.
- Commande de pompes.
- Gestion des autorisations.
- Interverrouillages.
- Signalisation logique.
- Gestion de certaines alarmes.

Exemple de logique :

`KM_Pompe1 = CMD_Marche ET Autorisation ET NON Defaut_Thermique`

Cette expression signifie que le contacteur de la pompe peut être commandé lorsque la demande de marche et l'autorisation sont présentes, sans défaut thermique.

### 3.2. FBD — Function Block Diagram

FBD représente le programme sous forme de blocs fonctionnels reliés entre eux.

Il est adapté :
- Aux fonctions logiques.
- Aux traitements analogiques.
- Aux comparaisons.
- Aux opérations arithmétiques.
- À l'assemblage de fonctions existantes.

Exemple :

Un bloc ET reçoit :
- Une autorisation.
- Une demande de marche.
- Une information d'absence de défaut.

Sa sortie commande une variable logique.

### 3.3. SCL — Structured Control Language

SCL est un langage textuel Siemens fondé sur les principes du langage Structured Text (ST).

Il est particulièrement intéressant pour :
- Les calculs complexes.
- Les tableaux de données.
- Les boucles.
- Les traitements répétitifs.
- Les algorithmes.
- Les traitements conditionnels complexes.

Exemple : calcul de la puissance active triphasée.

Pour un réseau triphasé équilibré :

`P = √3 × U × I × cos φ`

Avec :
- P : puissance active en watts.
- U : tension composée en volts.
- I : courant en ampères.
- cos φ : facteur de puissance.

Prenons :
- U = 400 V.
- I = 10 A.
- cos φ = 0,85.

Résultat :

`P = 1,732 × 400 × 10 × 0,85`

`P ≈ 5 889 W`

Soit environ 5,89 kW.

Exemple de code SCL simplifié :

```scl
Puissance_W := 1.732 * Tension_V * Courant_A * Facteur_Puissance;
Puissance_kW := Puissance_W / 1000.0;
```

Les variables utilisées doivent être déclarées dans l'interface du bloc avec des types adaptés, par exemple REAL.

### 3.4. GRAPH et les séquences

GRAPH est un outil Siemens destiné à la programmation séquentielle.

Il permet de représenter un processus sous forme :
- D'étapes.
- De transitions.
- D'actions associées aux étapes.

Il est utile lorsque le fonctionnement d'une machine suit un ordre précis.

Exemple : cycle d'une station de pompage.

| Étape | Fonction |
|---|---|
| E0 | Attente de la demande |
| E1 | Ouverture de la vanne |
| E2 | Démarrage de la pompe |
| E3 | Arrêt de la pompe et fermeture de la vanne |
| Retour E0 | Attente d'un nouveau cycle |

Une transition permet de passer d'une étape à la suivante lorsque sa condition est satisfaite.

GRAPH est un outil Siemens de programmation séquentielle apparenté aux principes des diagrammes fonctionnels séquentiels. Il ne faut toutefois pas considérer GRAPH comme un synonyme universel de SFC.

---

## 4. Tableau comparatif des langages

| Langage ou outil | Représentation | Utilisation privilégiée |
|---|---|---|
| LAD | Schéma à contacts | Commandes logiques |
| FBD | Blocs fonctionnels | Fonctions logiques et traitements par blocs |
| ST | Texte structuré | Calculs et algorithmes |
| SCL | Langage textuel Siemens fondé sur ST | Calculs, boucles et traitements complexes |
| GRAPH | Séquences graphiques Siemens | Processus séquentiels |
| IL | Liste d'instructions | Anciennes applications et maintenance de systèmes historiques |

**Remarque sur IL :** Instruction List est un ancien langage de la famille IEC 61131-3. Il a été retiré des éditions récentes de la norme. Il ne faut pas le considérer comme un langage de programmation disponible dans TIA Portal V17.

---

## 5. Utilisation de plusieurs langages dans un même projet

Il n'est pas nécessaire de choisir un seul langage pour tout le programme automate.

Dans TIA Portal, plusieurs blocs peuvent être développés dans des langages différents, selon les fonctions à réaliser et les possibilités de la CPU et de l'environnement logiciel.

### Exemple : station de pompage de quatre pompes

| Fonction | Langage conseillé | Justification |
|---|---|---|
| Commande des pompes | LAD | Logique Marche/Arrêt et autorisations |
| Calcul des courants moyens | SCL | Calculs et boucles |
| Gestion d'une séquence | GRAPH | Enchaînement d'étapes |
| Combinaisons de fonctions | FBD | Assemblage de blocs fonctionnels |

Cette organisation permet de profiter des avantages de chaque langage.

---

## 6. Organisation d'un programme automate

Un programme industriel doit être structuré pour que chaque partie possède une fonction identifiable.

Il est préférable d'éviter de concentrer toutes les instructions dans OB1.

### 6.1. Principe de modularité

La programmation modulaire consiste à répartir les différentes fonctions dans plusieurs blocs spécialisés.

Exemple d'organisation :

```text
                 OB1
                  |
       ┌──────────┼──────────┐
       |          |          |
       ▼          ▼          ▼
  FC_Commandes  FC_Calculs  FC_Alarmes
       |
   ┌───┼───┬───┐
   ▼   ▼   ▼   ▼
 Pompe1 Pompe2 Pompe3 Pompe4
```

Cette représentation est conceptuelle : l'organisation réelle des appels dépend de l'architecture retenue.

### 6.2. Exemple d'organisation d'une station de pompage

| Bloc | Responsabilité |
|---|---|
| OB1 | Coordination du programme cyclique |
| FC_Acquisition | Traitement des informations d'entrée |
| FC_Pompe1 | Gestion de la pompe 1 |
| FC_Pompe2 | Gestion de la pompe 2 |
| FC_Pompe3 | Gestion de la pompe 3 |
| FC_Pompe4 | Gestion de la pompe 4 |
| FC_Calculs | Calculs de mesures et de grandeurs |
| FC_Alarmes | Gestion des conditions d'alarme |
| DB_Parametres | Stockage des paramètres du système |

Cette organisation est un exemple. Le nombre et le type de blocs doivent être adaptés à la taille réelle de l'installation.

### 6.3. Avantages de la modularité

- Le programme est plus lisible.
- Le diagnostic est facilité.
- Les modifications sont plus simples.
- Les fonctions peuvent être réutilisées.
- Plusieurs automaticiens peuvent travailler plus facilement sur différentes parties du projet.
- Les erreurs sont plus faciles à localiser.

Il faut néanmoins éviter de créer un bloc pour chaque instruction élémentaire. Une modularisation excessive peut rendre le programme inutilement complexe.

---

## 7. Rôle des blocs OB, FC, FB et DB

### 7.1. OB — Organization Block

Les blocs OB organisent l'exécution du programme.

OB1 est généralement le bloc principal exécuté de manière cyclique dans un programme S7-1200 classique.

Il peut appeler les blocs qui réalisent les différentes fonctions de l'installation.

Il existe également d'autres types d'OB destinés à des événements ou traitements spécifiques.

### 7.2. FC — Function

Une FC est une fonction qui ne possède pas de mémoire statique propre.

Elle peut recevoir des paramètres et retourner un résultat.

Exemples :
- Calcul d'une moyenne.
- Conversion d'une valeur.
- Traitement d'une mesure.
- Calcul d'une puissance.

### 7.3. FB — Function Block

Un FB est un bloc fonctionnel qui dispose d'une mémoire d'instance pour conserver des données entre les appels.

Il est particulièrement adapté aux équipements possédant un comportement et des données propres.

Exemples :
- Bloc de gestion d'une pompe.
- Bloc de commande d'un moteur.
- Bloc de gestion d'une vanne.

Un DB d'instance est associé à l'utilisation d'un FB pour stocker ses données d'instance.

### 7.4. DB — Data Block

Un DB est destiné au stockage des données.

Exemples :
- Paramètres de fonctionnement.
- Consignes.
- Seuils d'alarme.
- États mémorisés.
- Données échangées avec une IHM.

### 7.5. Tableau récapitulatif

| Bloc | Fonction principale | Mémoire statique propre |
|---|---|---|
| OB | Organisation de l'exécution | Selon son interface et sa configuration |
| FC | Exécution d'une fonction | Non |
| FB | Exécution d'une fonction avec état mémorisé | Oui |
| DB | Stockage des données | Contient les données déclarées |

---

## 8. Conventions de nommage des variables

Une variable doit posséder un nom compréhensible.

Les noms tels que `M0.0`, `M0.1` ou `M0.2` peuvent correspondre à des adresses mémoire, mais ils ne décrivent pas clairement la fonction de l'information.

Il est préférable d'utiliser des noms explicites.

### 8.1. Exemples

| Nom peu explicite | Nom recommandé |
|---|---|
| M0.0 | CMD_Marche_Pompe1 |
| M0.1 | DEF_Thermique_Pompe1 |
| M0.2 | Etat_Pompe1 |

### 8.2. Préfixes possibles

| Préfixe | Signification | Exemple |
|---|---|---|
| BP_ | Bouton-poussoir | BP_Marche_Pompe1 |
| KM_ | Contacteur moteur | KM_Pompe1 |
| EV_ | Électrovanne | EV_Remplissage |
| CAP_ | Capteur | CAP_Pression |
| DEF_ | Défaut | DEF_Thermique_Pompe1 |
| ALM_ | Alarme | ALM_Pression_Basse |
| CMD_ | Commande | CMD_Marche_Pompe1 |
| MES_ | Mesure | MES_Temperature_Moteur |

Ces préfixes sont des conventions de projet. Ils ne sont pas imposés par Siemens.

### 8.3. Choix des types de données

Le nom d'une variable doit être accompagné d'un type adapté à l'information stockée.

| Variable | Information | Type conseillé |
|---|---|---|
| Etat_Pompe | Marche ou arrêt | BOOL |
| Temperature_Moteur | 75,8 °C | REAL |
| Nombre_Defauts | 125 défauts | INT |

- BOOL : information logique, TRUE ou FALSE.
- INT : nombre entier signé.
- REAL : nombre réel permettant de représenter des valeurs décimales.

Le type doit également tenir compte de la plage de valeurs attendue.

---

## 9. Commentaires et documentation

Un programme bien organisé doit être documenté.

Les commentaires doivent expliquer la fonction du programme et les conditions importantes de fonctionnement.

### 9.1. Commentaires des réseaux

Au lieu de conserver uniquement :

- Network 1
- Network 2
- Network 3

Utiliser des intitulés explicites :

- Réseau 1 — Commande de la pompe 1.
- Réseau 2 — Gestion du défaut thermique.
- Réseau 3 — Signalisation de marche.

### 9.2. Exemple de commentaire utile

« Ce réseau autorise le fonctionnement de la pompe lorsque la commande de marche est active et qu'aucun défaut thermique n'est présent. »

### 9.3. Bonnes pratiques

- Donner un nom explicite aux blocs.
- Donner un titre aux réseaux.
- Commenter les fonctions importantes.
- Décrire les variables.
- Documenter les paramètres.
- Éviter les commentaires inutiles ou obsolètes.
- Maintenir les commentaires à jour après les modifications.

Un programme peut compiler correctement tout en étant difficile à comprendre. La qualité d'un programme automate ne se limite donc pas à sa compilation.

---

## 10. Erreurs fréquentes dans l'organisation d'un programme

| Erreur | Conséquence | Bonne pratique |
|---|---|---|
| Tout programmer dans OB1 | Programme difficile à lire | Répartir les fonctions |
| Utiliser des noms ambigus | Diagnostic compliqué | Employer des noms explicites |
| Oublier les commentaires | Compréhension difficile | Documenter les fonctions |
| Mélanger plusieurs responsabilités dans un réseau | Maintenance difficile | Une responsabilité claire par réseau |
| Utiliser un type inadapté | Erreurs de calcul ou de traitement | Choisir le type selon la donnée |
| Répéter inutilement du code | Modifications répétitives | Réutiliser les blocs lorsque pertinent |
| Créer trop de petits blocs | Architecture inutilement complexe | Modulariser avec discernement |

---

## 11. Application industrielle : station de pompage à quatre pompes

Considérons une station comportant quatre pompes identiques.

Le programme doit assurer :
- La commande indépendante des pompes.
- Le traitement des mesures.
- Le calcul des grandeurs nécessaires.
- La gestion des défauts.
- La transmission des informations à l'IHM.

Une organisation possible est :

```text
PROGRAMME AUTOMATE
│
├── OB1
│   ├── FC_Acquisition
│   ├── FC_Pompe1
│   ├── FC_Pompe2
│   ├── FC_Pompe3
│   ├── FC_Pompe4
│   ├── FC_Calculs
│   └── FC_Alarmes
│
└── DB_Parametres
```

Exemples de variables :

| Variable | Type | Fonction |
|---|---|---|
| BP_Marche_Pompe1 | BOOL | Demande de marche |
| BP_Arret_Pompe1 | BOOL | Demande d'arrêt |
| DEF_Thermique_Pompe1 | BOOL | Défaut thermique |
| Etat_Pompe1 | BOOL | État logique de la pompe |
| MES_Courant_Pompe1 | REAL | Courant mesuré |
| MES_Temperature_Moteur | REAL | Température mesurée |
| ALM_Pression_Basse | BOOL | Alarme de pression basse |
| Nombre_Defauts | INT | Nombre de défauts comptabilisés |

Cette architecture est une proposition de structuration. Les protections réelles, les conditions de sécurité et les automatismes de l'installation doivent être définis selon les exigences techniques du site.

**Important :** une fonction d'arrêt d'urgence ne doit pas reposer uniquement sur une logique standard LAD dans un automate non adapté à la sécurité. Elle nécessite une architecture de sécurité appropriée.

---

## 12. QCM corrigé

**Résultat obtenu : 7/7.**

Les notions évaluées étaient les suivantes :

| N° | Notion évaluée | Bonne réponse | Explication |
|---|---|---|---|
| 1 | Commande simple de moteur | LAD | Une commande Marche/Arrêt avec autorisations repose sur une logique de contacts. |
| 2 | Calcul d'une puissance triphasée | SCL | Les calculs et expressions arithmétiques sont adaptés à SCL. |
| 3 | Somme de mesures contenues dans un tableau | SCL | Les tableaux et boucles sont particulièrement adaptés au traitement textuel. |
| 4 | Organisation des fonctions | Blocs spécialisés | La modularité facilite la maintenance. |
| 5 | Type d'une température décimale | REAL | REAL permet de représenter des valeurs décimales. |
| 6 | Rôle d'OB1 | Exécution cyclique principale | OB1 organise généralement le cycle principal du programme. |
| 7 | Importance de la modularité | Maintenance et travail en équipe | Une architecture claire facilite les évolutions et le diagnostic. |

---

## 13. Exercices corrigés

### Exercice 1 — Choix du langage

Associer chaque fonction au langage adapté.

| Fonction | Réponse | Correction |
|---|---|---|
| 1. Commande d'un moteur | LAD | Correct |
| 2. Calcul d'une grandeur électrique | SCL | Correct |
| 3. Gestion d'une séquence de fonctionnement | GRAPH | Correct |
| 4. Assemblage de blocs logiques | FBD | Correct |

**Résultat : 4/4.**

### Exercice 2 — Organisation des blocs

Une station de pompage possède quatre pompes. OB1 contient actuellement toutes les instructions de commande, de calcul et de gestion des alarmes.

Quelle amélioration faut-il proposer ?

**Bonne réponse : C. Répartir les fonctions dans plusieurs blocs spécialisés, appelés depuis OB1.**

Explication : cette organisation améliore la lisibilité, le diagnostic et la maintenance.

**Résultat : 1/1.**

### Exercice 3 — Choix des types de données

| Variable | Réponse | Correction |
|---|---|---|
| Etat_Pompe | BOOL | Correct |
| Temperature_Moteur | REAL | Correct |
| Nombre_Defauts | INT | Correct |

Explication :
- BOOL représente deux états logiques.
- REAL représente des nombres décimaux.
- INT représente des nombres entiers signés.

**Résultat : 3/3.**

### Exercice 4 — Documentation du programme

Un programme contient uniquement des noms génériques pour les réseaux, des variables peu explicites et aucun commentaire.

**Bonne réponse : B. Renommer les réseaux, utiliser des noms de variables explicites et ajouter des commentaires utiles.**

Explication : la documentation facilite la compréhension du programme, le dépannage et les interventions de maintenance.

**Résultat : 1/1.**

### Bilan des exercices

| Exercice | Résultat |
|---|---:|
| Exercice 1 | 4/4 |
| Exercice 2 | 1/1 |
| Exercice 3 | 3/3 |
| Exercice 4 | 1/1 |
| **Total** | **9/9** |

---

## 14. Bilan des travaux pratiques

Les manipulations pratiques liées à l'organisation des programmes et à l'utilisation des blocs dans TIA Portal ont été déclarées réalisées.

Les travaux déjà effectués dans la formation comprennent notamment :
- La création et l'utilisation de blocs LAD.
- La réalisation de réseaux logiques.
- La création et la compilation d'un bloc SCL.
- La découverte de FBD.
- La découverte de GRAPH.
- L'organisation des programmes à l'aide de blocs.
- L'utilisation de noms de variables et de commentaires.

Aucune reprise de ces manipulations n'est nécessaire dans cette leçon.

---

## 15. Conclusion générale

Le choix d'un langage de programmation doit dépendre de la fonction industrielle à réaliser.

LAD est particulièrement adapté aux commandes logiques, FBD aux assemblages de blocs fonctionnels, SCL aux calculs et algorithmes, et GRAPH aux processus séquentiels.

Un programme industriel de qualité doit également être organisé de manière modulaire, avec des blocs clairement identifiés, des variables explicites et une documentation suffisante.

La combinaison de ces bonnes pratiques améliore la lisibilité, la maintenance, le diagnostic et l'évolution des installations automatisées.

**Leçon 8 terminée et validée.**

- QCM : 7/7.
- Exercices : 9/9.
- Travaux pratiques : réalisés.
