# Leçon 4 — Architecture matérielle de l'automate Siemens S7-1200

## 1. Présentation générale

Le Siemens S7-1200 est un automate programmable industriel utilisé pour commander des machines et des procédés industriels.

Il reçoit les informations provenant des capteurs, traite ces informations dans sa CPU et commande les équipements de l'installation.

## 2. La CPU (Central Processing Unit)

La CPU constitue l'unité centrale de l'automate.

Ses principales fonctions sont :

- Lire les informations provenant des entrées.
- Exécuter le programme utilisateur.
- Effectuer les opérations logiques et mathématiques.
- Commander les sorties.
- Gérer les communications avec les autres équipements.

Exemple : dans une station de pompage, la CPU analyse les informations des capteurs de niveau et commande le démarrage ou l'arrêt des pompes.

## 3. Les entrées et sorties de l'automate

### 3.1 Entrées numériques (DI)

DI signifie Digital Input.

Elles reçoivent des informations logiques provenant notamment :

- Des boutons-poussoirs.
- Des contacts auxiliaires.
- Des détecteurs de proximité.
- Des pressostats.
- Des fins de course.

Les états logiques sont généralement représentés par 0 et 1.

### 3.2 Sorties numériques (DQ)

DQ signifie Digital Output.

Elles permettent de commander des équipements tels que :

- Des relais.
- Des contacteurs.
- Des voyants.
- Des électrovannes.

Le type de sortie (relais ou transistor notamment) et ses caractéristiques électriques doivent être vérifiés avant le raccordement.

### 3.3 Entrées analogiques (AI)

AI signifie Analog Input.

Elles reçoivent des signaux électriques variables provenant de transmetteurs ou de capteurs adaptés.

Exemples :
- 0–10 V.
- 4–20 mA.
- 0–20 mA.

Exemple : un transmetteur de pression 0–10 bar fournit un signal 4–20 mA à l'automate.

### 3.4 Sorties analogiques (AQ)

AQ signifie Analog Output.

Elles délivrent des signaux électriques variables permettant de commander certains équipements.

Exemples :
- Consigne 0–10 V vers un variateur.
- Consigne 4–20 mA vers un équipement compatible.

## 4. Tableau récapitulatif des E/S

| Abréviation | Signification | Fonction | Exemple |
|---|---|---|---|
| DI | Digital Input | Recevoir une information logique | Bouton Marche |
| DQ | Digital Output | Commander un équipement | Contacteur |
| AI | Analog Input | Recevoir une mesure variable | Transmetteur de pression |
| AQ | Analog Output | Fournir une consigne variable | Variateur de vitesse |

À retenir :
- Input : information qui entre dans l'automate.
- Output : commande qui sort de l'automate.
- Digital : numérique.
- Analog : analogique.

## 5. Le port Ethernet et PROFINET

Le S7-1200 possède une interface Ethernet industrielle permettant notamment :

- La programmation depuis TIA Portal.
- La communication avec une IHM.
- Les échanges avec d'autres automates.
- La communication avec certains variateurs.
- L'échange de données avec des systèmes de supervision.

PROFINET est un protocole de communication industrielle basé sur Ethernet.

Il faut distinguer :
- Ethernet : technologie de réseau.
- PROFINET : protocole industriel.
- Modbus TCP : autre protocole industriel utilisant Ethernet.
- Adresse IP : identifiant logique d'un équipement sur un réseau IP.
- Adresse MAC : identifiant matériel d'une interface réseau.

## 6. L'alimentation de l'automate

Les CPU S7-1200 existent avec différentes caractéristiques d'alimentation selon leur référence.

Certaines sont alimentées en 24 V DC, tandis que d'autres utilisent une alimentation AC.

Une alimentation 230 V AC / 24 V DC peut fournir le 24 V continu nécessaire à une CPU prévue pour cette tension.

Il faut toujours vérifier la référence exacte de la CPU avant son raccordement.

Le programme peut rester enregistré dans la mémoire de l'automate après une coupure électrique, mais son exécution nécessite une alimentation adaptée.

## 7. Les modules d'extension

Lorsque les entrées et sorties intégrées à la CPU sont insuffisantes, il est possible d'ajouter des modules compatibles.

Exemples de familles de modules :

| Module | Fonction |
|---|---|
| SM 1221 | Entrées numériques |
| SM 1222 | Sorties numériques |
| SM 1231 | Entrées analogiques |
| SM 1232 | Sorties analogiques |
| SM 1234 | Entrées et sorties analogiques |

La compatibilité dépend de la référence de la CPU, de sa génération et des caractéristiques du module.

### Exemple de dimensionnement

Une installation nécessite :
- 14 entrées numériques.
- 10 sorties numériques.

Une CPU possédant 8 DI et 6 DQ intégrées peut être complétée, si elle est compatible, par :
- Un module de 8 DI.
- Un module de 4 DQ.

Le système disposera alors de :
- 16 entrées numériques.
- 10 sorties numériques.

## 8. Configuration des modules dans TIA Portal

L'ajout physique d'un module ne suffit pas.

Il faut également :

1. Vérifier la compatibilité matérielle.
2. Ajouter le module dans la configuration des appareils de TIA Portal.
3. Vérifier les adresses des entrées et sorties.
4. Compiler la configuration matérielle.
5. Charger la configuration dans l'automate.

La simulation dépend des capacités de la version de PLCSIM et du matériel configuré.

## 9. Exemple d'application industrielle

Dans une station de pompage, nous pouvons retrouver :

| Équipement | Type de signal | Type de voie |
|---|---|---|
| Bouton Marche | TOR | DI |
| Détecteur de niveau | TOR | DI |
| Transmetteur de pression 4–20 mA | Analogique | AI |
| Commande contacteur | TOR | DQ |
| Consigne variateur 0–10 V | Analogique | AQ |

TOR signifie Tout Ou Rien.

## 10. À retenir

La CPU traite les informations et exécute le programme utilisateur.

Les entrées reçoivent les informations du terrain et les sorties transmettent les commandes.

Les modules d'extension permettent d'adapter les capacités de l'automate aux besoins de l'installation.

La référence exacte du matériel doit toujours être vérifiée avant toute configuration ou tout raccordement électrique.
