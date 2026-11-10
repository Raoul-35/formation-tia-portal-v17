# Leçon 9 — Les types de données dans un automate Siemens S7-1200

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre le rôle des types de données dans un automate programmable.
- Distinguer les données logiques, entières, décimales et temporelles.
- Identifier les principaux types de données du S7-1200.
- Connaître leur taille et leurs plages de valeurs.
- Choisir un type adapté à une variable industrielle.
- Comprendre l'importance du choix des types dans TIA Portal V17.

## 2. Définition d'un type de données

Un type de données indique à l'automate comment représenter, interpréter et traiter une information.

Chaque variable déclarée dans un programme automate possède un type de données.

Exemples dans une station de pompage :

| Information | Exemple de valeur |
|---|---|
| Pompe en marche | TRUE |
| Niveau de la cuve | 75.5 % |
| Température moteur | 68.4 °C |
| Nombre de démarrages | 125 |
| Temps de fonctionnement | 2 heures |
| Défaut thermique | TRUE |

Ces informations ne peuvent pas toutes être représentées de la même manière.

## 3. Les types logiques

### 3.1 BOOL

BOOL représente une information logique qui possède deux états :

- TRUE : vrai, état logique 1.
- FALSE : faux, état logique 0.

Taille : 1 bit.

Exemples :

- Pompe_En_Marche
- Defaut_Thermique
- BP_Marche
- Mode_Automatique
- Niveau_Haut

Le type BOOL est très utilisé dans les programmes LAD pour les contacts, les bobines et les conditions logiques.

## 4. Les types de données binaires

BYTE, WORD et DWORD représentent des groupes de bits.

| Type | Taille | Nombre de bits |
|---|---:|---:|
| BYTE | 8 bits | 8 |
| WORD | 16 bits | 16 |
| DWORD | 32 bits | 32 |

Lorsqu'ils sont interprétés comme des nombres non signés, leurs plages de valeurs sont :

| Type | Valeur minimale | Valeur maximale |
|---|---:|---:|
| BYTE | 0 | 255 |
| WORD | 0 | 65 535 |
| DWORD | 0 | 4 294 967 295 |

Exemples d'utilisation :

- BYTE : groupe de 8 états binaires.
- WORD : mot d'état d'un équipement.
- DWORD : groupe de 32 informations binaires.

Attention : ces types représentent d'abord des groupes de bits. Ils ne doivent pas être confondus avec les types entiers non signés.

## 5. Les types entiers non signés

Les types entiers non signés représentent des nombres entiers positifs ou nuls.

| Type | Taille | Valeur minimale | Valeur maximale |
|---|---:|---:|---:|
| USINT | 8 bits | 0 | 255 |
| UINT | 16 bits | 0 | 65 535 |
| UDINT | 32 bits | 0 | 4 294 967 295 |

Exemples :

- USINT : petit compteur.
- UINT : nombre de démarrages d'une pompe.
- UDINT : compteur nécessitant une grande capacité.

## 6. Les types entiers signés

Les types entiers signés permettent de représenter des valeurs positives, négatives et nulles.

| Type | Taille | Valeur minimale | Valeur maximale |
|---|---:|---:|---:|
| INT | 16 bits | -32 768 | +32 767 |
| DINT | 32 bits | -2 147 483 648 | +2 147 483 647 |

Exemple industriel :

Une variable représente l'écart entre une température mesurée et une température de référence.

- +5 : température supérieure de 5 °C.
- -3 : température inférieure de 3 °C.

Le type INT convient à cette application.

## 7. Le type REAL

REAL permet de représenter des nombres en virgule flottante.

Taille : 32 bits, soit 4 octets.

Exemples :

- Niveau_Cuve = 75.5 %
- Temperature_Moteur = 68.4 °C
- Pression = 6.25 bar
- Intensite = 12.7 A

REAL est adapté aux mesures industrielles comportant des parties décimales.

Attention : la représentation binaire des nombres flottants peut entraîner de petites imprécisions sur certaines valeurs décimales.

## 8. Le type TIME

TIME représente une durée en millisecondes.

Taille : 32 bits.

Exemples de notation :

| Valeur | Signification |
|---|---|
| T#1S | 1 seconde |
| T#5S | 5 secondes |
| T#30S | 30 secondes |
| T#2M | 2 minutes |
| T#1H | 1 heure |
| T#2M30S | 2 minutes et 30 secondes |

Exemple industriel :

Pour imposer une temporisation de 10 secondes à un temporisateur TON :

PT = T#10S

Signification des principales entrées et sorties d'un TON :

- IN : entrée d'activation.
- PT : durée programmée.
- ET : durée écoulée.
- Q : sortie de fin de temporisation.

La temporisation se termine lorsque la durée programmée est atteinte, si IN reste à TRUE.

## 9. Tableau récapitulatif

| Type | Taille | Utilisation |
|---|---:|---|
| BOOL | 1 bit | État logique |
| BYTE | 8 bits | Groupe de bits |
| WORD | 16 bits | Groupe de bits |
| DWORD | 32 bits | Groupe de bits |
| USINT | 8 bits | Entier non signé |
| UINT | 16 bits | Entier non signé |
| UDINT | 32 bits | Entier non signé |
| INT | 16 bits | Entier signé |
| DINT | 32 bits | Entier signé |
| REAL | 32 bits | Nombre décimal |
| TIME | 32 bits | Durée |

## 10. Application industrielle : station de pompage

Voici les types adaptés à quelques variables :

| Nom de variable | Description | Type |
|---|---|---|
| Pompe_En_Marche | État de la pompe | BOOL |
| Temperature_Moteur | Température en °C | REAL |
| Nombre_Demarrages | Nombre de démarrages | UINT |
| Ecart_Niveau | Écart positif ou négatif | INT |
| Temps_Demarrage | Durée de démarrage | TIME |

## 11. Points essentiels à retenir

1. Chaque variable d'un automate possède un type de données.
2. BOOL représente deux états logiques.
3. BYTE, WORD et DWORD représentent des groupes de bits.
4. USINT, UINT et UDINT représentent des entiers non signés.
5. INT et DINT représentent des entiers signés.
6. REAL représente des nombres décimaux.
7. TIME représente une durée.
8. Le choix du type dépend de la nature de l'information et de sa plage de valeurs.

## 12. Conclusion

Le choix correct des types de données constitue une étape fondamentale dans la programmation d'un automate programmable industriel.

Il permet de représenter correctement les informations, d'effectuer les traitements adaptés et d'éviter certaines erreurs de programmation.

Cette connaissance sera utilisée dans les prochains modules pour configurer les variables, programmer les blocs LAD et développer les interfaces WinCC Advanced.
