# Leçon 7 — Introduction à la configuration matérielle du S7-1200

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Expliquer la configuration matérielle d'un automate.
- Identifier les principaux composants matériels d'un S7-1200.
- Comprendre le rôle de la CPU et des modules d'extension.
- Distinguer les entrées/sorties intégrées des entrées/sorties déportées.
- Comprendre les notions de référence matérielle et de compatibilité.
- Expliquer l'importance de la configuration matérielle dans TIA Portal.

## 2. Définition de la configuration matérielle

La configuration matérielle consiste à représenter dans TIA Portal les équipements physiques qui constituent une installation automatisée.

Elle permet notamment de déclarer :

- Le modèle de la CPU.
- Les modules d'entrées et de sorties.
- Les interfaces de communication.
- Les modules d'extension.
- L'organisation des différents équipements.

Exemple : une station de pompage peut comporter un automate S7-1200, des capteurs de niveau, un transmetteur de pression, des contacteurs, des variateurs et une IHM.

La configuration matérielle permet de représenter l'architecture de commande de cette installation dans le logiciel.

## 3. La CPU du S7-1200

La CPU (Central Processing Unit) constitue l'unité centrale de traitement de l'automate.

Ses principales fonctions sont :

1. Exécuter le programme utilisateur.
2. Traiter les informations provenant des entrées.
3. Effectuer les opérations logiques et mathématiques.
4. Commander les sorties.
5. Gérer les communications.
6. Assurer certaines fonctions de diagnostic.

### 3.1 CPU étudiée dans notre formation

Notre formation utilise :

**SIMATIC S7-1200 — CPU 1214C DC/DC/DC — Firmware V4.5.**

Signification :

- CPU 1214C : modèle de l'unité centrale.
- Premier DC : alimentation en courant continu.
- Deuxième DC : entrées numériques en courant continu.
- Troisième DC : sorties numériques à transistor fonctionnant en courant continu.
- Firmware V4.5 : version du logiciel interne de la CPU.

Attention : les sorties à transistor sont différentes des sorties à relais. Elles présentent des contraintes électriques particulières.

## 4. Les entrées et sorties

Les entrées permettent à l'automate de recevoir des informations provenant du procédé industriel.

Les sorties permettent à l'automate de transmettre des ordres aux équipements de commande.

### 4.1 Entrées numériques (DI)

DI signifie Digital Input.

Une entrée numérique reçoit généralement deux états logiques :

- 0 : état logique bas.
- 1 : état logique haut.

Exemples :

- Bouton-poussoir Marche.
- Bouton-poussoir Arrêt.
- Détecteur de présence.
- Capteur de niveau TOR.
- Contact auxiliaire d'un contacteur.

### 4.2 Sorties numériques (DQ)

DQ signifie Digital Output.

Une sortie numérique transmet un ordre logique à un équipement.

Exemples :

- Commande d'un contacteur.
- Commande d'une électrovanne.
- Activation d'un voyant.
- Commande TOR d'un équipement compatible.

Les sorties doivent respecter les caractéristiques électriques des récepteurs raccordés.

### 4.3 Entrées analogiques (AI)

AI signifie Analog Input.

Une entrée analogique reçoit un signal représentant une grandeur physique variable.

Exemples de signaux :

- 0–10 V.
- 4–20 mA.

Exemples de mesures :

- Pression.
- Température.
- Niveau.
- Débit.

Exemple industriel : un transmetteur de pression délivre un signal 4–20 mA correspondant à la pression mesurée dans une conduite.

### 4.4 Sorties analogiques (AQ)

AQ signifie Analog Output.

Une sortie analogique transmet une consigne variable à un équipement compatible.

Exemples :

- Consigne de vitesse d'un variateur.
- Consigne de position d'une vanne.
- Consigne de régulation.

## 5. Les modules d'extension

Lorsque les entrées et sorties intégrées à la CPU sont insuffisantes, il est possible d'ajouter des modules d'extension compatibles.

Exemples de familles de modules S7-1200 :

| Famille | Fonction |
|---|---|
| SM 1221 | Entrées numériques |
| SM 1222 | Sorties numériques |
| SM 1231 | Entrées analogiques |
| SM 1232 | Sorties analogiques |
| SM 1234 | Entrées et sorties analogiques |

Le choix d'un module doit tenir compte :

- De sa compatibilité avec la CPU.
- De ses caractéristiques électriques.
- Du nombre de voies nécessaires.
- Des types de signaux acceptés.
- Des limites de configuration du système.

Il ne suffit donc pas de choisir un module uniquement en fonction de son nom.

## 6. Entrées/sorties intégrées et déportées

### 6.1 Entrées/sorties intégrées

Elles sont directement présentes sur la CPU.

Exemple : un bouton-poussoir raccordé à une entrée numérique intégrée de la CPU.

### 6.2 Entrées/sorties déportées

Elles sont installées à distance de la CPU et communiquent avec l'automate à travers un réseau industriel.

Exemple : une station d'E/S déportées installée près des moteurs et des capteurs d'une usine.

Avantages :

- Réduction des longueurs de câbles ramenés à l'armoire principale.
- Répartition des équipements.
- Adaptation aux installations industrielles étendues.

Attention : les modules d'extension locaux installés à côté de la CPU et les stations d'E/S déportées ne représentent pas la même architecture.

## 7. Importance de la configuration matérielle dans TIA Portal

La configuration matérielle permet à TIA Portal de connaître :

- Le modèle exact de la CPU.
- La version du firmware.
- Les modules présents.
- Les caractéristiques des entrées et sorties.
- Les interfaces de communication.
- Les adresses matérielles affectées aux E/S.

Ces informations sont indispensables pour préparer correctement la programmation, la compilation et le chargement du projet.

Exemple : une entrée numérique peut être associée à l'adresse `%I0.0`, mais l'adresse réelle dépend de la configuration de l'automate et de ses modules.

Une erreur de configuration peut provoquer des problèmes lors de la compilation, du chargement ou du fonctionnement de l'installation.

## 8. Exemple d'application industrielle

Une station de pompage possède :

- Quatre boutons-poussoirs.
- Deux capteurs de niveau TOR.
- Un transmetteur de pression 4–20 mA.
- Deux contacteurs de pompes.
- Une CPU S7-1200 CPU 1214C DC/DC/DC.
- Un module SM 1231.

Classement :

| Catégorie | Équipements |
|---|---|
| DI | Quatre boutons-poussoirs et deux capteurs de niveau TOR |
| AI | Transmetteur de pression 4–20 mA |
| DQ | Commandes des deux contacteurs de pompes |
| CPU | S7-1200 CPU 1214C DC/DC/DC |
| Module d'extension | SM 1231 |

Le module SM 1231 permet d'ajouter des entrées analogiques. Il peut recevoir le transmetteur de pression si sa référence exacte prend en charge le signal 4–20 mA.

Les sorties numériques commandent généralement les bobines des contacteurs, en respectant leurs caractéristiques électriques et les protections nécessaires.

## 9. Évaluation des connaissances

### 9.1 Résultat du QCM

| Question | Réponse correcte | Résultat |
|---|---|---|
| 1. Rôle de la configuration matérielle | B | Correct |
| 2. Signification DC/DC/DC | B | Correct |
| 3. Fonction du SM 1231 | C | Correct |
| 4. Différence DI et AI | B | Correct |
| 5. Rôle des E/S déportées | B | Correct |
| 6. Compatibilité matérielle | C | Correct |

**Résultat : 6/6.**

### 9.2 Résultat de l'exercice d'application

Classement réalisé :

- DI : quatre boutons-poussoirs et deux capteurs de niveau TOR.
- AI : un transmetteur de pression 4–20 mA.
- DQ : deux contacteurs de pompes.
- CPU : S7-1200 CPU 1214C DC/DC/DC.
- Module d'extension : SM 1231.

**Résultat : 5/5.**

## 10. Compte rendu des manipulations pratiques

Les manipulations pratiques relatives à la configuration matérielle ont déjà été réalisées dans TIA Portal V17.

L'environnement de travail utilisé est :

- Logiciel : TIA Portal V17.
- Projet : Formation_TIA_Portal_V17.
- Automate : S7-1200 CPU 1214C DC/DC/DC.
- Firmware : V4.5.

Les manipulations ont notamment porté sur la découverte et l'exploration de la configuration matérielle dans TIA Portal.

Conformément à la progression pédagogique, les manipulations ne sont pas répétées dans cette séance.

## 11. Synthèse

La configuration matérielle constitue une étape fondamentale dans la préparation d'un projet d'automatisation.

Elle permet de représenter les équipements physiques, de déclarer la CPU, d'ajouter les modules nécessaires et de définir les interfaces d'entrée, de sortie et de communication.

La maîtrise de cette étape facilite la programmation, la compilation, le diagnostic et la maintenance des installations automatisées.
