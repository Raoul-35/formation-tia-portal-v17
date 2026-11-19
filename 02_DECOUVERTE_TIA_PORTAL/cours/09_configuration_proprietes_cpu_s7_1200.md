# LEÇON 9 — CONFIGURATION DES PROPRIÉTÉS DE LA CPU S7-1200

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Identifier les principales propriétés d'une CPU S7-1200.
- Configurer les paramètres de communication PROFINET.
- Comprendre l'adressage IP et le masque de sous-réseau.
- Distinguer le nom de l'appareil dans TIA Portal du nom PROFINET.
- Comprendre les paramètres de démarrage de la CPU.
- Comprendre le principe de rémanence des données.
- Identifier les fonctions de protection d'accès.
- Utiliser les fonctions de diagnostic de la CPU.

---

## 2. Présentation générale des propriétés de la CPU

Dans TIA Portal, les propriétés de la CPU permettent de configurer différents paramètres matériels et logiciels qui déterminent son fonctionnement.

On peut notamment y retrouver :

- Les informations générales de la CPU.
- Les paramètres PROFINET.
- Les paramètres d'adressage IP.
- Les paramètres de démarrage.
- Les paramètres de rémanence.
- Les paramètres de protection et de sécurité.
- Les informations de diagnostic.

Ces paramètres sont importants pour assurer le bon fonctionnement de l'automate et sa communication avec les autres équipements industriels.

## 3. Propriétés générales de la CPU

Les propriétés générales permettent notamment de consulter ou de configurer :

- Le nom de l'appareil dans le projet.
- La référence matérielle.
- La version du firmware.
- Les commentaires et informations descriptives.

### Exemple

Dans notre projet :

- Nom de l'automate : PLC_1.
- Famille : S7-1200.
- CPU : 1214C DC/DC/DC.
- Firmware : V4.5.

Il faut distinguer la référence matérielle de la version du firmware.

La référence matérielle identifie le modèle précis de la CPU, tandis que le firmware correspond au logiciel interne qui permet à la CPU de fonctionner.

## 4. Configuration de la communication PROFINET

### 4.1. Définition de PROFINET

PROFINET est un système de communication industrielle basé sur Ethernet.

Il permet notamment de connecter :

- Les automates programmables.
- Les interfaces homme-machine (IHM).
- Les périphériques d'entrées/sorties déportées.
- Les équipements industriels compatibles.
- Les ordinateurs d'ingénierie.

### 4.2. Adresse IP

Une adresse IP permet d'identifier un équipement sur un réseau informatique.

Une adresse IPv4 est constituée de 32 bits, généralement représentés sous la forme de quatre nombres décimaux séparés par des points.

Exemple :

192.168.0.1

Dans notre projet, la CPU utilise :

| Paramètre | Valeur |
|---|---|
| Adresse IP | 192.168.0.1 |
| Masque | 255.255.255.0 |
| Passerelle | Non configurée |

### 4.3. Masque de sous-réseau

Le masque permet de déterminer la partie de l'adresse IP correspondant au réseau et celle correspondant à l'équipement.

Avec le masque :

255.255.255.0

Les trois premiers octets identifient le réseau dans cet exemple.

Ainsi :

- CPU : 192.168.0.1
- PC : 192.168.0.10

Les deux équipements appartiennent au même sous-réseau.

Attention : deux équipements d'un même réseau doivent avoir des adresses IP distinctes.

### 4.4. Passerelle

La passerelle permet de communiquer avec d'autres réseaux.

Dans une connexion directe entre un PC et une CPU sur le même sous-réseau, une passerelle n'est généralement pas nécessaire.

## 5. Nom de l'appareil et nom PROFINET

Il faut distinguer deux notions.

### Nom de l'appareil dans TIA Portal

Il permet d'identifier l'appareil dans l'arborescence du projet.

Exemple :

PLC_1

### Nom d'appareil PROFINET

Il permet d'identifier l'équipement sur le réseau PROFINET.

Exemple :

automate_pompage

Le nom PROFINET et l'adresse IP sont deux paramètres différents et complémentaires.

## 6. Paramètres de démarrage de la CPU

Une CPU S7-1200 possède différents états de fonctionnement.

### STOP

Dans cet état :

- Le programme utilisateur cyclique n'est pas exécuté.
- OB1 n'est pas exécuté.
- Les sorties prennent les états configurés pour STOP.
- La CPU reste alimentée.
- La communication avec TIA Portal peut rester possible.

### STARTUP

C'est une phase transitoire qui intervient lors du démarrage de la CPU.

Certains blocs de démarrage, comme OB100, peuvent être exécutés lorsqu'ils sont configurés.

### RUN

Dans cet état :

- Le programme utilisateur est exécuté.
- OB1 fonctionne de manière cyclique.
- Les instructions de commande sont traitées.

### Précaution industrielle

Dans une station de pompage, le passage automatique de STOP à RUN peut entraîner le redémarrage des pompes si le programme le prévoit.

Il faut donc prévoir une logique de redémarrage maîtrisée et vérifier les conditions de sécurité avant toute remise en marche.

## 7. Rémanence des données

### 7.1. Définition

La rémanence est la capacité de certaines variables à conserver leur valeur après une coupure d'alimentation ou un redémarrage de la CPU.

Toutes les variables ne sont pas automatiquement rémanentes.

Le comportement dépend notamment :

- Du type de mémoire utilisé.
- De la configuration des variables.
- Des caractéristiques de la CPU.
- Des paramètres du bloc de données concerné.

### 7.2. Exemple industriel

Une pompe possède une variable :

Nombre_Demarrages_Pompe

Type : UINT

Avant une coupure électrique :

Nombre_Demarrages_Pompe = 250

Si cette variable est correctement configurée comme rémanente, sa valeur pourra être conservée après le redémarrage.

Cette fonction est particulièrement utile pour :

- Les compteurs de fonctionnement.
- Les compteurs de démarrages.
- Certaines consignes de production.
- Certaines informations de suivi de maintenance.

### 7.3. Différence entre Temp et Static

Dans un bloc FB :

- Temp : variables temporaires utilisées pendant l'exécution du bloc.
- Static : variables dont les valeurs sont conservées entre les appels grâce au DB d'instance.

Attention : une variable Static n'est pas automatiquement rémanente après une coupure d'alimentation. Sa rémanence doit être configurée lorsque cela est nécessaire.

## 8. Protection et sécurité d'accès

La protection d'accès permet de limiter les opérations autorisées sur la CPU.

Elle contribue à protéger le programme et les paramètres de l'automate contre les modifications non autorisées.

Selon les paramètres disponibles, on peut notamment contrôler :

- Les droits de consultation.
- Les possibilités de modification.
- Les opérations de configuration.
- L'accès en ligne.
- L'authentification des utilisateurs.

### Exemple industriel

Dans une usine :

- Un opérateur peut consulter certaines informations.
- Un automaticien habilité peut modifier le programme.
- Un responsable technique peut gérer les paramètres de protection.

La protection d'accès ne remplace pas les mesures de cybersécurité du réseau industriel.

## 9. Diagnostic de la CPU

Les fonctions de diagnostic permettent d'identifier et d'analyser les problèmes rencontrés par l'automate.

Elles peuvent notamment signaler :

- Les défauts matériels.
- Les problèmes de communication.
- Les défauts des modules d'extension.
- Les événements de démarrage.
- Certaines erreurs d'exécution.
- Les informations de diagnostic des équipements.

### Exemple industriel

Une CPU S7-1200 est en RUN, mais l'IHM ne communique plus avec elle.

Le technicien doit notamment vérifier :

1. L'adresse IP de la CPU.
2. L'adresse IP de l'IHM.
3. Le masque de sous-réseau.
4. Les connexions Ethernet.
5. La configuration de la liaison HMI.
6. Les informations de diagnostic.

Une CPU en RUN ne garantit pas que toutes les communications fonctionnent correctement.

## 10. Synthèse de la leçon

| Élément | Fonction principale |
|---|---|
| Nom de l'appareil | Identifier l'appareil dans le projet |
| Référence matérielle | Identifier le modèle de CPU |
| Firmware | Logiciel interne de la CPU |
| Adresse IP | Identifier l'équipement sur le réseau IP |
| Masque | Déterminer le sous-réseau |
| Passerelle | Permettre la communication avec d'autres réseaux |
| Nom PROFINET | Identifier l'équipement sur le réseau PROFINET |
| Paramètres de démarrage | Définir le comportement au démarrage |
| Rémanence | Conserver certaines données |
| Protection d'accès | Contrôler les droits d'accès |
| Diagnostic | Identifier et analyser les défauts |

---

# 11. ÉVALUATION THÉORIQUE — QCM CORRIGÉ

### Question 1
Quel est le rôle principal des propriétés de la CPU ?

**Réponse B :** Configurer différents paramètres de fonctionnement, de communication et de protection de la CPU.

### Question 2
À quoi sert l'adresse IP ?

**Réponse B :** Identifier la CPU sur le réseau Ethernet pour permettre les communications.

### Question 3
Quelle est la différence entre le nom TIA Portal et le nom PROFINET ?

**Réponse C :** Le nom TIA Portal identifie l'appareil dans le projet d'ingénierie, tandis que le nom PROFINET sert à identifier l'équipement sur le réseau PROFINET.

### Question 4
À quoi sert la rémanence ?

**Réponse A :** Conserver certaines données après une coupure d'alimentation ou un redémarrage.

### Question 5
Quel est l'objectif de la protection d'accès ?

**Réponse B :** Contrôler les droits d'accès et limiter les opérations autorisées sur l'automate.

### Question 6
Quelle fonction permet d'identifier les problèmes matériels et de communication ?

**Réponse C :** Les fonctions de diagnostic.

### Question 7
Pourquoi faut-il être vigilant avec le démarrage automatique en RUN ?

**Réponse B :** Parce que les pompes peuvent redémarrer automatiquement si les conditions de sécurité ne sont pas correctement gérées.

**Résultat : 7/7 — 100 %.**

---

# 12. EXERCICES D'APPLICATION CORRIGÉS

### Exercice 1 — Configuration réseau

La CPU possède :

- IP : 192.168.0.1
- Masque : 255.255.255.0

Quelle adresse IP peut être attribuée au PC ?

**Réponse : 192.168.0.10**

Explication : le PC appartient au même sous-réseau que la CPU, mais possède une adresse différente.

### Exercice 2 — Rémanence

On souhaite conserver le compteur de démarrages d'une pompe après une coupure électrique.

**Réponse :** Configurer la variable comme rémanente dans les paramètres appropriés.

### Exercice 3 — Diagnostic

Une CPU est en RUN, mais l'IHM ne communique plus avec elle.

**Réponse :** Consulter les informations de diagnostic et vérifier les paramètres de communication réseau.

### Exercice 4 — Sécurité du redémarrage

Après une coupure électrique, une pompe ne doit pas redémarrer sans vérification des conditions de fonctionnement.

**Réponse :** Vérifier les conditions d'autorisation et de sécurité avant d'autoriser le redémarrage.

**Résultat : 4/4 — 100 %.**

---

# 13. COMPTE RENDU DE LA PRATIQUE

## Environnement utilisé

- Logiciel : TIA Portal V17.
- Projet : Formation_TIA_Portal_V17.
- Automate : Siemens S7-1200 CPU 1214C DC/DC/DC.
- Firmware : V4.5.

## Manipulations réalisées

Les manipulations pratiques de configuration des propriétés de la CPU ont déjà été réalisées dans TIA Portal.

Elles ont notamment porté sur la découverte et la configuration des paramètres suivants :

- Propriétés générales de la CPU.
- Paramètres de communication PROFINET.
- Adressage IP.
- Paramètres de démarrage.
- Rémanence des données.
- Protection d'accès.
- Fonctions de diagnostic.

Aucune répétition de ces manipulations n'est nécessaire dans le cadre de cette leçon.

## Conclusion pratique

Cette manipulation a permis de mieux comprendre l'importance des propriétés de la CPU dans la configuration d'un automate industriel.

La configuration matérielle ne se limite pas à l'ajout de la CPU et des modules d'extension. Elle comprend également les paramètres de communication, de fonctionnement, de conservation des données et de protection.

---

# 14. CONCLUSION GÉNÉRALE

La configuration des propriétés de la CPU constitue une étape essentielle dans la préparation d'un projet d'automatisation industrielle.

Elle permet d'adapter le comportement de l'automate aux exigences de l'installation, de faciliter les communications avec les équipements périphériques et de renforcer la fiabilité du système.

La maîtrise de ces paramètres est indispensable avant de passer à des applications plus avancées de programmation et de supervision industrielle.
