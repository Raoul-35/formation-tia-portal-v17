# MODULE 07 – COMMUNICATION INDUSTRIELLE

# SÉQUENCE 01 – FONDAMENTAUX ET ARCHITECTURES DES RÉSEAUX INDUSTRIELS

## 1. Objectifs pédagogiques

À la fin de cette séquence, l'apprenant doit être capable de :

- Définir la communication industrielle et expliquer son importance.
- Identifier les différents niveaux d'une architecture industrielle.
- Comprendre les principes de transmission des données numériques.
- Distinguer les modes de transmission et les supports physiques.
- Identifier les principales topologies réseau.
- Comprendre les modèles de communication industriels.
- Interpréter les performances d'un réseau industriel.
- Proposer une architecture de communication adaptée à une installation automatisée.

## 2. Généralités sur la communication industrielle

### 2.1 Définition

La communication industrielle désigne l'ensemble des moyens matériels et logiciels permettant l'échange d'informations entre les équipements d'une installation industrielle.

Ces équipements peuvent être :

- Des capteurs et des transmetteurs ;
- Des automates programmables industriels (API) ;
- Des variateurs de vitesse ;
- Des interfaces homme-machine (IHM) ;
- Des systèmes de supervision ;
- Des ordinateurs industriels ;
- Des systèmes de gestion et de maintenance.

### 2.2 Rôle de la communication industrielle

Elle permet notamment :

- La transmission des mesures des capteurs vers les automates.
- L'envoi des commandes vers les équipements.
- L'échange de données entre plusieurs automates.
- La supervision des installations.
- La remontée des alarmes et des défauts.
- L'exploitation des données de production et de maintenance.

### 2.3 Exemple industriel

Dans une station de pompage automatisée :

1. Le transmetteur de pression mesure la pression du réseau.
2. L'automate reçoit et traite cette information.
3. L'automate commande les pompes selon la logique programmée.
4. L'IHM affiche la pression, les états des pompes et les alarmes.
5. L'opérateur peut consulter les informations et envoyer des commandes autorisées.

La communication assure donc la circulation des informations entre les différents équipements.

## 3. Architecture des réseaux industriels

Une installation industrielle peut être organisée en plusieurs niveaux fonctionnels.

### 3.1 Niveau terrain

Il regroupe les équipements directement liés au procédé :

- Capteurs ;
- Détecteurs ;
- Transmetteurs ;
- Actionneurs ;
- Variateurs de vitesse.

Il fournit les informations nécessaires au contrôle du procédé et reçoit les commandes.

### 3.2 Niveau commande

Il regroupe principalement :

- Les automates programmables ;
- Les contrôleurs industriels ;
- Les systèmes de commande.

L'automate traite les informations provenant du terrain et élabore les commandes destinées aux équipements.

### 3.3 Niveau supervision

Il comprend notamment :

- Les interfaces homme-machine ;
- Les systèmes SCADA ;
- Les postes opérateurs.

Il permet de visualiser le fonctionnement de l'installation, de consulter les alarmes et d'interagir avec le système automatisé.

### 3.4 Niveau gestion

Il regroupe les systèmes informatiques destinés à l'exploitation globale de l'entreprise :

- Gestion de production ;
- Gestion de maintenance ;
- Gestion des stocks ;
- Systèmes ERP.

Ces systèmes exploitent les informations provenant des niveaux inférieurs.

## 4. Principes de transmission des données

### 4.1 Information analogique et numérique

Une information analogique peut prendre une infinité de valeurs dans une plage donnée.

Exemple : un transmetteur de pression délivrant un signal 4–20 mA.

Une information numérique est représentée par des états discrets.

Exemples :

- 0 ou 1 ;
- Marche ou arrêt ;
- Défaut ou absence de défaut.

Les communications numériques transportent les informations sous forme de données codées.

### 4.2 Bit et octet

Le bit est la plus petite unité d'information numérique. Il peut prendre deux valeurs : 0 ou 1.

Un octet est constitué de 8 bits.

Exemple :

`01000001`

Cette suite contient 8 bits, soit un octet.

### 4.3 Types de données

Dans un automate, les informations peuvent être représentées par différents types :

| Type | Description | Exemple |
|---|---|---|
| BOOL | Valeur booléenne | Pompe en marche |
| INT | Entier signé | Nombre de démarrages |
| REAL | Nombre réel | Pression = 3,5 bar |
| BYTE | Groupe de 8 bits | État regroupé |
| WORD | Groupe de 16 bits | Mot de données |
| DWORD | Groupe de 32 bits | Valeur numérique |

### 4.4 Transmission série et parallèle

**Transmission série :**

Les bits sont transmis successivement sur une liaison.

Avantages :
- Moins de conducteurs ;
- Câblage généralement plus simple ;
- Adaptée aux communications sur de longues distances selon la technologie.

**Transmission parallèle :**

Plusieurs bits sont transmis simultanément sur plusieurs conducteurs.

Avantages :
- Transmission simultanée de plusieurs bits.

Limites :
- Nombre important de conducteurs ;
- Sensibilité aux différences de synchronisation entre les signaux ;
- Utilisation plus limitée dans les réseaux industriels modernes.

## 5. Modes de communication

### 5.1 Simplex

La communication s'effectue dans un seul sens.

Exemple : un équipement transmet des informations vers un système de réception sans recevoir de réponse sur cette liaison.

### 5.2 Half-duplex

La communication peut s'effectuer dans les deux sens, mais pas simultanément.

Les équipements alternent entre émission et réception.

### 5.3 Full-duplex

Les deux équipements peuvent transmettre et recevoir simultanément.

Ce mode améliore la capacité d'échange lorsque la technologie et l'architecture le permettent.

## 6. Supports physiques de communication

### 6.1 Paire torsadée en cuivre

Elle est constituée de conducteurs torsadés permettant de limiter les perturbations électromagnétiques.

Utilisations :
- RS-485 ;
- Ethernet industriel ;
- Certains réseaux de terrain.

### 6.2 Fibre optique

Elle transporte les données sous forme de signaux lumineux.

Avantages :
- Immunité aux perturbations électromagnétiques ;
- Adaptation aux longues distances ;
- Isolation électrique entre les équipements.

Limites :
- Coût des équipements optiques ;
- Nécessité de compétences et d'outils adaptés pour le raccordement.

### 6.3 Communication sans fil

Elle utilise les ondes radio pour transmettre les informations.

Exemples :
- Wi-Fi industriel ;
- Bluetooth dans certaines applications ;
- Réseaux radio industriels.

Avantages :
- Réduction du câblage ;
- Mobilité ;
- Possibilité de communiquer avec des équipements difficiles à raccorder.

Limites :
- Perturbations radioélectriques ;
- Couverture à garantir ;
- Contraintes de sécurité et de disponibilité.

## 7. Topologies des réseaux industriels

La topologie représente la manière dont les équipements sont organisés et reliés entre eux.

### 7.1 Topologie en bus

Tous les équipements sont raccordés à une même ligne de communication.

Avantages :
- Câblage relativement simple ;
- Réduction du nombre de câbles.

Limites :
- Une défaillance de la ligne principale peut perturber plusieurs équipements ;
- Respect des règles de terminaison nécessaire selon la technologie.

### 7.2 Topologie en étoile

Les équipements sont raccordés à un équipement central, généralement un commutateur réseau.

Avantages :
- Gestion simple des connexions ;
- Isolation possible d'une liaison défectueuse ;
- Extension facilitée.

Limite :
- Une défaillance de l'équipement central peut interrompre les communications.

### 7.3 Topologie en anneau

Les équipements sont reliés de manière à former une boucle.

Avantages :
- Possibilité de redondance selon le protocole utilisé ;
- Continuité de communication possible après une rupture si le mécanisme de redondance est prévu.

### 7.4 Topologie en arbre

Elle associe plusieurs niveaux de connexions hiérarchiques.

Elle convient aux installations comportant plusieurs zones ou ateliers.

## 8. Modèles de communication

### 8.1 Maître-esclave

Un équipement maître organise les échanges avec un ou plusieurs équipements esclaves.

Le maître interroge les équipements et organise généralement les communications.

Ce modèle est utilisé dans plusieurs systèmes industriels.

### 8.2 Client-serveur

Le client demande un service ou une donnée. Le serveur répond à cette demande.

Exemple :

Un système de supervision demande à un automate une valeur de pression. L'automate fournit la donnée selon le mécanisme de communication utilisé.

### 8.3 Pair-à-pair (Peer-to-Peer)

Deux équipements peuvent échanger directement des informations sans qu'un équipement central soit nécessairement responsable de tous les échanges.

Ce modèle peut être utilisé pour les communications entre contrôleurs.

## 9. Performances d'un réseau industriel

### 9.1 Débit

Le débit représente la quantité de données transmise pendant une durée donnée.

Il est généralement exprimé en bit/s.

Exemples :
- kbit/s ;
- Mbit/s ;
- Gbit/s.

Un débit élevé permet de transmettre davantage de données par unité de temps.

### 9.2 Latence

La latence représente le délai entre l'émission d'une information et son arrivée à destination.

Une faible latence est importante pour les applications nécessitant des échanges rapides.

### 9.3 Temps de réponse

Le temps de réponse correspond au délai nécessaire pour qu'un système réagisse à une demande ou à un événement.

Il peut inclure :
- Le temps de transmission ;
- Le temps de traitement ;
- Le temps de réponse de l'équipement destinataire.

### 9.4 Disponibilité

La disponibilité représente la capacité d'un système à rester opérationnel et accessible.

Une bonne disponibilité est essentielle pour les installations industrielles fonctionnant en continu.

### 9.5 Déterminisme

Un réseau déterministe permet de maîtriser le délai de transmission des informations.

Cette caractéristique est importante pour les applications de commande et de synchronisation industrielle.

Un réseau ayant un débit élevé n'est donc pas automatiquement adapté à toutes les applications temps réel.

## 10. Application à la station de pompage automatisée

Notre projet de station de pompage comporte notamment :

- Un automate Siemens S7-1200 ;
- Deux pompes ;
- Des informations de pression ;
- Des retours de fonctionnement ;
- Des informations de défaut ;
- Une interface de supervision WinCC.

### 10.1 Architecture fonctionnelle

**Niveau terrain :**
- Mesure de pression ;
- Retours de marche des pompes ;
- Informations de défaut des moteurs.

**Niveau commande :**
- Automate S7-1200 ;
- Traitement des informations ;
- Gestion automatique et manuelle des pompes ;
- Gestion des alarmes et des sécurités.

**Niveau supervision :**
- WinCC Runtime Advanced ;
- Affichage de la pression ;
- Affichage des états des pompes ;
- Commandes opérateur ;
- Consultation des alarmes.

### 10.2 Schéma fonctionnel simplifié

Capteurs et retours d'état  
↓  
Automate S7-1200  
↓  
Traitement de la logique de commande  
↓  
Commandes des pompes  

En parallèle :

Automate S7-1200  
↔  
WinCC Runtime Advanced

L'échange entre l'automate et WinCC permettra à l'opérateur de visualiser et de commander les fonctions autorisées.

### 10.3 Limites de notre environnement de formation

Notre formation utilise TIA Portal V17 et PLCSIM, sans automate physique ni équipements industriels raccordés.

Nous pouvons donc :
- Étudier les architectures ;
- Comprendre les mécanismes de communication ;
- Configurer les communications prises en charge par l'environnement logiciel ;
- Tester certains échanges en simulation.

En revanche, les essais physiques sur câbles, équipements réseau, automates réels et instruments de terrain nécessiteront du matériel adapté.

## 11. Synthèse des notions essentielles

| Notion | Signification |
|---|---|
| Communication industrielle | Échange d'informations entre équipements industriels |
| Niveau terrain | Capteurs et actionneurs |
| Niveau commande | Automates et contrôleurs |
| Niveau supervision | IHM et SCADA |
| Transmission série | Bits transmis successivement |
| Transmission parallèle | Plusieurs bits transmis simultanément |
| Simplex | Communication dans un seul sens |
| Half-duplex | Communication dans les deux sens, alternativement |
| Full-duplex | Communication simultanée dans les deux sens |
| Topologie bus | Équipements raccordés à une ligne commune |
| Topologie étoile | Équipements raccordés à un point central |
| Latence | Délai de transmission |
| Déterminisme | Maîtrise des délais de communication |

## 12. Évaluation des connaissances

L'évaluation comporte 10 questions.

| Question | Réponse correcte | Notion évaluée |
|---|---|---|
| 1 | B | Rôle de la communication industrielle |
| 2 | C | Architecture industrielle |
| 3 | C | Données numériques |
| 4 | B | Transmission série |
| 5 | B | Transmission et communication industrielle |
| 6 | B | Supports de transmission |
| 7 | B | Topologies réseau |
| 8 | C | Modèles de communication |
| 9 | A | Performances réseau |
| 10 | B | Déterminisme |

**Résultat obtenu : 10/10**

**Statut : SÉQUENCE VALIDÉE**

## 13. Conclusion

Cette première séquence a permis d'acquérir les bases nécessaires à la compréhension des réseaux industriels.

La maîtrise des architectures, des supports de transmission, des topologies et des performances constitue un préalable à l'étude des protocoles industriels.

La prochaine séquence portera sur les réseaux et protocoles industriels, notamment RS-232, RS-485, Modbus RTU, PROFIBUS, Ethernet industriel et PROFINET.
