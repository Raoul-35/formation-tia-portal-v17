# Leçon 2 — Chaîne d'information et chaîne d'énergie

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Distinguer la chaîne d'information de la chaîne d'énergie.
- Identifier les fonctions de chaque chaîne.
- Reconnaître les équipements qui interviennent dans chacune d'elles.
- Comprendre comment les deux chaînes collaborent dans un système automatisé.

## 2. La chaîne d'information

La chaîne d'information représente l'ensemble des fonctions permettant de recueillir des informations, de les traiter et de les communiquer.

Elle comprend principalement trois fonctions :

### 2.1 Acquérir

Cette fonction consiste à recueillir des informations provenant du procédé industriel.

Exemples :
- Capteur de niveau.
- Capteur de température.
- Détecteur de présence.
- Capteur de pression.

### 2.2 Traiter

Cette fonction consiste à analyser les informations reçues et à élaborer des décisions.

L'automate programmable industriel (API), comme le Siemens S7-1200, assure généralement cette fonction.

Exemple : si le niveau d'eau devient inférieur au seuil défini, l'automate peut décider de démarrer une pompe.

### 2.3 Communiquer

Cette fonction consiste à transmettre les informations vers d'autres équipements ou systèmes.

Exemples :
- Communication entre un automate et une IHM.
- Communication entre automates.
- Transmission de données par un réseau industriel.

L'IHM permet également à l'opérateur de visualiser les informations et d'interagir avec le système.

## 3. La chaîne d'énergie

La chaîne d'énergie représente l'ensemble des fonctions permettant d'alimenter un système en énergie et de réaliser une action physique.

Elle comprend généralement cinq fonctions.

### 3.1 Alimenter

Fournir l'énergie nécessaire au fonctionnement du système.

Exemple : réseau électrique triphasé alimentant une station de pompage.

### 3.2 Distribuer

Diriger ou autoriser le passage de l'énergie vers les équipements concernés.

Exemples :
- Contacteur.
- Disjoncteur.
- Variateur de vitesse.

Le contacteur permet notamment de commander électriquement l'alimentation du moteur.

### 3.3 Convertir

Transformer une forme d'énergie en une autre.

Exemple : un moteur électrique transforme l'énergie électrique en énergie mécanique.

### 3.4 Transmettre

Transmettre l'énergie mécanique vers l'organe chargé de réaliser l'action.

Exemples :
- Accouplement mécanique.
- Réducteur.
- Courroie et poulies.

### 3.5 Agir

Réaliser l'action attendue sur le procédé industriel.

Exemple : une pompe met l'eau en mouvement.

## 4. Exemple industriel : station de pompage

Considérons une station de pompage équipée :
- D'un capteur de niveau.
- D'un automate Siemens S7-1200.
- D'un contacteur.
- D'un moteur électrique.
- D'une pompe.
- D'une IHM.

### 4.1 Chaîne d'information

1. Le capteur détecte un niveau d'eau bas.
2. Il transmet cette information à l'entrée de l'automate.
3. L'automate traite l'information.
4. Il décide de démarrer la pompe.
5. Il peut également transmettre l'état du système à l'IHM.

### 4.2 Chaîne d'énergie

1. Le réseau triphasé fournit l'énergie électrique.
2. Le contacteur permet de distribuer cette énergie au moteur.
3. Le moteur transforme l'énergie électrique en énergie mécanique.
4. L'accouplement transmet le mouvement à la pompe.
5. La pompe met l'eau en circulation.

## 5. Interaction entre les deux chaînes

Les deux chaînes sont complémentaires.

La chaîne d'information permet au système de savoir ce qui se passe et de prendre des décisions.

La chaîne d'énergie permet d'exécuter physiquement les décisions prises.

Dans notre exemple, l'automate décide de démarrer la pompe, mais c'est la chaîne d'énergie qui permet réellement à celle-ci de fonctionner.

## 6. Tableau récapitulatif

| Chaîne | Fonction | Exemple d'équipement |
|---|---|---|
| Information | Acquérir | Capteur de niveau |
| Information | Traiter | Automate S7-1200 |
| Information | Communiquer | Réseau industriel |
| Énergie | Alimenter | Réseau triphasé |
| Énergie | Distribuer | Contacteur |
| Énergie | Convertir | Moteur électrique |
| Énergie | Transmettre | Accouplement |
| Énergie | Agir | Pompe |

## 7. Conclusion

Dans un système automatisé industriel, la chaîne d'information et la chaîne d'énergie travaillent ensemble.

La première recueille et traite les informations afin de déterminer les actions à effectuer.

La seconde fournit et transforme l'énergie nécessaire à la réalisation de ces actions.

La maîtrise de ces deux chaînes constitue une base essentielle pour comprendre la programmation des automates et la conception des systèmes automatisés.
