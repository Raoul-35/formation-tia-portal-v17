# Leçon 3 — Les différents types d'automates programmables industriels (API)

## 1. Définition d'un API

Un Automate Programmable Industriel (API) est un équipement électronique programmable utilisé pour commander automatiquement des machines et des procédés industriels.

Il reçoit des informations provenant des capteurs, exécute un programme utilisateur et commande les actionneurs.

### Exemple : station de pompage

- Un capteur détecte le niveau d'eau.
- L'API reçoit cette information.
- L'API exécute son programme.
- Il commande le contacteur de la pompe.
- La pompe démarre ou s'arrête selon les conditions programmées.

## 2. Les principales architectures d'API

### 2.1 API compact

Un API compact regroupe généralement plusieurs éléments dans un même boîtier :

- Unité centrale (CPU).
- Entrées et sorties intégrées.
- Alimentation, selon le modèle.

Avantages :
- Installation relativement simple.
- Encombrement réduit.
- Adapté aux petites installations.

Limites :
- Possibilités d'extension parfois limitées.
- Remplacement de l'ensemble parfois nécessaire en cas de panne de la CPU.

Exemples :
- Siemens LOGO!
- Schneider Modicon M221.

### 2.2 API modulaire

Un API modulaire est constitué de plusieurs modules assemblés selon les besoins de l'installation.

Principaux modules :
- CPU.
- Alimentation.
- Entrées numériques.
- Sorties numériques.
- Entrées analogiques.
- Sorties analogiques.
- Communication.

Avantages :
- Grande souplesse de configuration.
- Possibilité d'extension.
- Remplacement individuel des modules.

Exemples :
- Siemens S7-1500.
- Siemens S7-300.
- Schneider Modicon M580.

### 2.3 Architecture avec entrées/sorties distribuées

Dans une architecture distribuée, les entrées et sorties peuvent être installées à différents endroits de l'usine.

Elles communiquent avec l'automate par un réseau industriel.

Exemple :
- Un automate principal dans une armoire électrique.
- Une station d'E/S déportées près d'une pompe.
- Une autre station près d'un convoyeur.

Avantages :
- Réduction des longueurs de câbles de terrain.
- Répartition des équipements.
- Facilitation de l'extension de l'installation.

Exemple de matériel :
- Siemens ET 200SP.

## 3. Comparaison des architectures

| Critères | API compact | API modulaire | E/S distribuées |
|---|---|---|---|
| Construction | Éléments regroupés | Modules assemblés | Modules répartis |
| Extension | Selon le modèle | Importante | Selon l'architecture |
| Installation | Simple | Plus complexe | Réseau industriel nécessaire |
| Utilisation | Petites machines | Installations complexes | Installations réparties |
| Maintenance | Relativement simple | Remplacement par module | Diagnostic réseau et modules |

## 4. Quelques gammes d'automates Siemens

| Gamme | Utilisation générale |
|---|---|
| LOGO! | Petites automatisations |
| S7-1200 | Machines et installations de petite à moyenne taille |
| S7-1500 | Applications industrielles plus avancées |
| S7-300 | Ancienne génération modulaire encore présente dans l'industrie |
| S7-400 | Applications industrielles de grande envergure |

## 5. Une distinction technique importante

Les architectures compactes, modulaires et distribuées ne sont pas nécessairement exclusives.

Par exemple :

- Un S7-1200 possède une CPU compacte, mais accepte des modules d'extension.
- Un S7-1500 possède une architecture modulaire.
- Un S7-1500 peut communiquer avec des stations d'E/S déportées ET 200.

Il faut donc distinguer l'architecture physique de l'automate et l'organisation des entrées/sorties dans l'installation.

## 6. Application à notre formation

Nous utiliserons principalement :

- Automate : Siemens S7-1200.
- Logiciel de programmation : TIA Portal V17.
- Simulation automate : S7-PLCSIM.
- Supervision : WinCC Advanced.
- Simulation IHM : WinCC Runtime Advanced.

Notre objectif sera de réaliser des applications industrielles complètes sans disposer d'un automate physique.

## 7. À retenir

Un API reçoit les informations des capteurs, traite ces informations à l'aide de son programme et commande les actionneurs.

Le choix de son architecture dépend notamment :
- Du nombre d'entrées et de sorties.
- De la complexité de l'installation.
- Des possibilités d'extension.
- Des besoins de communication.
- Des contraintes de maintenance.
