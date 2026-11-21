# Module 03 – Configuration de l'API S7-1200

# Leçon 2 : Configuration et adressage des entrées et sorties

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre le principe d'adressage des entrées et sorties d'un automate S7-1200.
- Distinguer les adresses des entrées numériques, sorties numériques et voies analogiques.
- Comprendre la correspondance entre les bornes physiques et les adresses utilisées dans le programme.
- Configurer et vérifier les adresses dans TIA Portal V17.
- Créer des variables symboliques pour faciliter la programmation.
- Identifier les risques de chevauchement d'adresses.

## 2. Pourquoi adresser les entrées et sorties ?

Un automate programmable industriel communique avec les équipements installés sur une machine.

Exemple d'une station de pompage :

| Équipement | Type de signal | Adresse illustrative |
|---|---|---|
| Bouton Marche | Entrée numérique | `%I0.0` |
| Bouton Arrêt | Entrée numérique | `%I0.1` |
| Niveau bas | Entrée numérique | `%I0.2` |
| Niveau haut | Entrée numérique | `%I0.3` |
| Contacteur pompe | Sortie numérique | `%Q0.0` |
| Voyant marche | Sortie numérique | `%Q0.1` |

Les adresses permettent au programme de retrouver les informations correspondant aux bornes physiques de l'automate.

Les adresses présentées dans les exemples sont illustratives. Les adresses réelles dépendent de la CPU et de la configuration matérielle.

## 3. Structure des adresses numériques

### 3.1. Entrées numériques : %I

La lettre I signifie Input (entrée).

Exemple : `%I0.3`

- I : entrée.
- 0 : numéro de l'octet.
- 3 : numéro du bit dans cet octet.

Un octet contient 8 bits, numérotés de 0 à 7.

| Adresse | Signification |
|---|---|
| `%I0.0` | Bit 0 de l'octet 0 |
| `%I0.1` | Bit 1 de l'octet 0 |
| `%I0.2` | Bit 2 de l'octet 0 |
| `%I0.3` | Bit 3 de l'octet 0 |
| `%I0.4` | Bit 4 de l'octet 0 |
| `%I0.5` | Bit 5 de l'octet 0 |
| `%I0.6` | Bit 6 de l'octet 0 |
| `%I0.7` | Bit 7 de l'octet 0 |

L'adresse `%I1.0` désigne le bit 0 de l'octet 1.

### 3.2. Sorties numériques : %Q

La lettre Q signifie Output (sortie).

Exemples :

- `%Q0.0` : bit 0 de l'octet 0 des sorties.
- `%Q0.1` : bit 1 de l'octet 0.
- `%Q0.2` : bit 2 de l'octet 0.

Ces adresses permettent au programme de commander des actionneurs ou des préactionneurs.

## 4. Adressage des entrées et sorties analogiques

### 4.1. Entrées analogiques : %IW

Une entrée analogique fournit une valeur numérique généralement représentée sur 16 bits.

Exemple : `%IW64`.

- I : entrée.
- W : Word (mot de 16 bits).
- 64 : adresse de départ de la zone mémoire.

Les entrées analogiques permettent notamment de lire :

- Une pression.
- Une température.
- Un niveau.
- Un débit.

Exemple : un transmetteur de pression 4–20 mA raccordé à un module d'entrée analogique.

### 4.2. Sorties analogiques : %QW

Exemple : `%QW64`.

- Q : sortie.
- W : Word de 16 bits.
- 64 : adresse de départ.

Une sortie analogique peut transmettre une consigne à un variateur de vitesse.

Attention : les adresses analogiques dépendent du matériel et de la configuration des modules.

## 5. Les différentes représentations d'une adresse

Une même zone mémoire peut être représentée à différents niveaux.

| Représentation | Taille | Exemple |
|---|---:|---|
| BIT | 1 bit | `%I0.0` |
| BYTE | 8 bits | `%IB0` |
| WORD | 16 bits | `%IW0` |
| DWORD | 32 bits | `%ID0` |

Pour les sorties :

- `%Q0.0` : bit.
- `%QB0` : octet.
- `%QW0` : mot.
- `%QD0` : double mot.

### 5.1. Risque de chevauchement

Les adresses `%IB0` et `%IW0` désignent des zones mémoire qui se recouvrent.

- `%IB0` occupe 8 bits.
- `%IW0` occupe 16 bits, soit les octets 0 et 1.

Les deux adresses partagent donc l'octet 0.

Le chevauchement n'est pas systématiquement interdit, mais il peut provoquer des incohérences si les variables sont utilisées indépendamment.

Il faut toujours vérifier les zones mémoire utilisées.

## 6. Adressage absolu et adressage symbolique

### 6.1. Adressage absolu

L'adressage absolu consiste à utiliser directement une adresse.

Exemple : `%I0.0`.

### 6.2. Adressage symbolique

L'adressage symbolique consiste à associer un nom compréhensible à une adresse.

| Nom symbolique | Adresse | Type |
|---|---|---|
| `BP_Marche_Pompe` | `%I0.0` | Bool |
| `BP_Arret_Pompe` | `%I0.1` | Bool |
| `Niveau_Haut` | `%I0.3` | Bool |
| `Commande_Pompe` | `%Q0.0` | Bool |
| `Pression_Cuve` | `%IW64` | Int |

Avantages :

- Programme plus lisible.
- Maintenance facilitée.
- Identification rapide des fonctions.
- Réduction des erreurs de compréhension.

## 7. Correspondance entre le matériel et le programme

La chaîne de fonctionnement peut être représentée ainsi :

```text
PARTIE OPERATIVE
       |
Bouton-poussoir
       |
       v
BORNE PHYSIQUE
       |
       v
ADRESSE AUTOMATE
     %I0.0
       |
       v
PROGRAMME API
       |
Traitement logique
       |
       v
ADRESSE SORTIE
     %Q0.0
       |
       v
BORNE PHYSIQUE
       |
       v
CONTACTEUR
       |
       v
POMPE

Une adresse ne remplace pas le câblage électrique. Elle permet au programme d'identifier l'information correspondant à une borne physique.
8. Configuration des adresses dans TIA Portal V17
Les adresses des entrées et sorties sont définies dans la configuration matérielle.

Selon le matériel, il est possible de consulter ou de configurer :
- Les adresses de début des entrées.
- Les adresses de début des sorties.
- Les plages d'adresses des modules analogiques.
- Les paramètres propres aux voies analogiques.
Il faut toujours vérifier les adresses attribuées par TIA Portal au lieu de supposer qu'un module commence à une adresse donnée.
9. QCM corrigé
Question 1
Dans %I0.3, le chiffre 3 représente le numéro du bit dans l'octet.

Réponse : B.
Question 2
%Q0.2 représente le bit 2 de l'octet 0 des sorties.

Réponse : B.
Question 3
Un WORD possède 16 bits.

Réponse : C.
Question 4
%IW64 représente une entrée analogique et %QW64 une sortie analogique.

Réponse : B.
Question 5
L'adressage symbolique rend le programme plus lisible et plus facile à maintenir.

Réponse : C.
Question 6
Le chevauchement signifie que deux variables peuvent désigner des zones mémoire qui se recouvrent.

Réponse : A.

Résultat du QCM : 6/6.
10. Exercices d'application corrigés
Exercice 1 – Identification des adresses
Équipement	Adresse
Bouton Marche	%I0.0
Bouton Arrêt	%I0.1
Niveau bas	%I0.2
Niveau haut	%I0.3
Contacteur pompe	%Q0.0
Voyant marche	%Q0.1


Question : quelle adresse correspond au détecteur de niveau haut ?

Réponse : %I0.3.
Exercice 2 – Dimensionnement des entrées
Une station possède :
- 3 boutons-poussoirs.
- 4 détecteurs de niveau.
- 2 pressostats TOR.
Calcul :

3 + 4 + 2 = 9 entrées numériques.

Les contacteurs et voyants sont commandés par des sorties numériques et ne sont donc pas comptabilisés dans les DI.

Réponse : 9 DI.
Exercice 3 – Entrée analogique
Un transmetteur de pression 4–20 mA est raccordé à un module d'entrée analogique.

Réponse : une adresse de type %IW....
Exercice 4 – Chevauchement des adresses
Variables déclarées :
- Mesure_Analogique : %IW64.
- Octet_Entree : %IB64.
Ces deux variables partagent l'octet 64.

Réponse : risque de chevauchement.
Exercice 5 – Adressage symbolique
L'adresse %I0.2 correspond au détecteur de niveau haut.

Nom symbolique pertinent : Detecteur_Niveau_Haut.

Résultat des exercices : 5/5.
11. Compte rendu de la pratique dans TIA Portal V17
La consultation et la vérification des adresses des entrées et sorties ont déjà été réalisées dans TIA Portal V17, dans le projet Formation_TIA_Portal_V17.

Matériel étudié :
- Automate : Siemens S7-1200.
- CPU : 1214C DC/DC/DC.
- Firmware : V4.5.
Les manipulations déjà effectuées ont permis de se familiariser avec :
- La configuration matérielle de la CPU.
- La consultation des entrées et sorties intégrées.
- L'identification des adresses dans TIA Portal.
- La relation entre les entrées/sorties physiques et les adresses utilisées dans le programme.
La manipulation pratique n'a pas été répétée.
12. Points essentiels à retenir
- %I désigne les entrées.
- %Q désigne les sorties.
- %IW désigne une entrée sur un mot de 16 bits.
- %QW désigne une sortie sur un mot de 16 bits.
- Un BYTE contient 8 bits.
- Un WORD contient 16 bits.
- Un DWORD contient 32 bits.
- L'adressage symbolique facilite la lecture et la maintenance du programme.
- Le chevauchement apparaît lorsque deux adresses désignent des zones mémoire communes.
- Les adresses réelles doivent être vérifiées dans la configuration matérielle de TIA Portal.
Fin de la leçon 2 – Module 03
