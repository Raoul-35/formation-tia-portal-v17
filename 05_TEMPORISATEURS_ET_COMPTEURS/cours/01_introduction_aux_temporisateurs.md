# MODULE 05 — TEMPORISATEURS ET COMPTEURS

# LEÇON 1 — INTRODUCTION AUX TEMPORISATEURS (TIMERS)

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre le rôle des temporisateurs dans un automate programmable industriel.
- Identifier les différents types de temporisateurs Siemens.
- Distinguer TON, TOF et TP.
- Comprendre les paramètres IN, PT, Q et ET.
- Interpréter le fonctionnement d'un temporisateur à partir d'un chronogramme.
- Identifier les applications industrielles des temporisateurs.
- Analyser le comportement d'un temporisateur dans différentes situations.

---

## 2. Généralités sur les temporisateurs

Un temporisateur, également appelé Timer, est une fonction permettant de gérer une durée dans un programme automate.

Il permet notamment de :
- Retarder le démarrage d'un équipement.
- Retarder l'arrêt d'un équipement.
- Maintenir une sortie active pendant une durée déterminée.
- Créer des impulsions de durée définie.
- Organiser certaines séquences de fonctionnement.

### Exemple industriel

Dans une station de pompage, nous souhaitons démarrer une pompe cinq secondes après la réception d'une demande de marche.

Sans temporisateur, la pompe pourrait démarrer immédiatement dès que les conditions logiques sont satisfaites.

Avec un temporisateur, le démarrage est retardé de cinq secondes.

Autre exemple : dans une installation industrielle, un ventilateur doit continuer à fonctionner pendant dix secondes après l'arrêt d'un moteur afin d'évacuer la chaleur.

Ces deux applications nécessitent des comportements de temporisation différents.

---

## 3. Les principaux types de temporisateurs

Dans TIA Portal V17, nous étudions principalement trois types de temporisateurs :

| Temporisateur | Signification | Fonction |
|---|---|---|
| TON | Timer ON-Delay | Retard à l'enclenchement |
| TOF | Timer OFF-Delay | Retard à la retombée |
| TP | Pulse Timer | Génération d'une impulsion |

### 3.1. TON — Retard à l'enclenchement

TON signifie Timer ON-Delay.

Il permet de retarder l'activation de sa sortie après l'activation de son entrée.

#### Fonctionnement

1. L'entrée IN passe à TRUE.
2. Le temporisateur commence à compter.
3. Le temps écoulé ET augmente.
4. Lorsque ET atteint la durée de consigne PT, la sortie Q passe à TRUE.
5. Si IN repasse à FALSE avant ou après la fin de la temporisation, le temporisateur se réinitialise.

#### Exemple industriel : démarrage retardé d'une pompe

Une pompe doit démarrer cinq secondes après la réception d'une demande de marche.

Configuration :

`PT = T#5S`

| Situation | IN | ET | Q |
|---|---|---|---|
| Aucune demande | FALSE | T#0S | FALSE |
| Demande depuis 2 s | TRUE | T#2S | FALSE |
| Demande depuis 4 s | TRUE | T#4S | FALSE |
| Demande depuis 5 s | TRUE | T#5S | TRUE |
| Demande supprimée | FALSE | T#0S | FALSE |

#### Chronogramme simplifié

```text
IN
       ____|‾‾‾‾‾‾‾‾‾
           |
           |<-- 5 s -->

Q
       __________|‾‾‾‾
```

**Point essentiel :** le TON nécessite que son entrée IN reste à TRUE pendant toute la durée de consigne. Si IN repasse à FALSE, le comptage est réinitialisé.

### 3.2. TOF — Retard à la retombée

TOF signifie Timer OFF-Delay.

Il permet de retarder la désactivation de sa sortie après la désactivation de son entrée.

#### Fonctionnement

1. L'entrée IN passe à TRUE.
2. La sortie Q passe à TRUE.
3. Lorsque IN repasse à FALSE, le temporisateur commence à compter.
4. Pendant la temporisation, Q reste à TRUE.
5. Lorsque le temps PT est écoulé, Q passe à FALSE.

Si IN repasse à TRUE pendant la temporisation, la sortie reste active et le comptage de désactivation est annulé.

#### Exemple industriel : ventilation après arrêt d'une machine

Un ventilateur doit continuer à fonctionner pendant quinze secondes après l'arrêt d'une machine.

Configuration :

`PT = T#15S`

| Situation | IN | Q |
|---|---|---|
| Machine en marche | TRUE | TRUE |
| Arrêt demandé | FALSE | TRUE |
| Arrêt depuis 5 s | FALSE | TRUE |
| Arrêt depuis 10 s | FALSE | TRUE |
| Arrêt depuis 15 s | FALSE | FALSE |

#### Chronogramme simplifié

```text
IN
       ‾‾‾‾‾‾‾|________
              |
              |<-- 15 s -->

Q
       ‾‾‾‾‾‾‾‾‾‾|_____
```

**Point essentiel :** le TOF retarde la désactivation de la sortie lorsque son entrée repasse à FALSE.

### 3.3. TP — Temporisateur à impulsion

TP signifie Pulse Timer.

Il permet de produire une impulsion de durée déterminée lorsqu'un front montant apparaît sur son entrée.

Un front montant correspond au passage de FALSE à TRUE.

#### Fonctionnement

1. L'entrée IN passe de FALSE à TRUE.
2. Le temporisateur détecte le front montant.
3. La sortie Q passe immédiatement à TRUE.
4. La sortie reste active pendant la durée PT.
5. Lorsque la durée est écoulée, Q repasse à FALSE.

L'impulsion se termine à l'expiration de PT, même si l'entrée reste à TRUE.

#### Exemple industriel : lampe de signalisation

Une lampe doit s'allumer pendant six secondes après l'appui sur un bouton.

Configuration :

`PT = T#6S`

| Situation | IN | Q |
|---|---|---|
| Avant l'appui | FALSE | FALSE |
| Appui sur le bouton | TRUE | TRUE |
| Après 3 s | TRUE | TRUE |
| Après 6 s | TRUE | FALSE |

#### Chronogramme simplifié

```text
IN
       ____|‾‾‾‾‾‾‾‾‾‾‾

Q
       ____|‾‾‾|________
            6 s
```

**Point essentiel :** le TP génère une impulsion de durée définie à partir d'un front montant.

---

## 4. Comparaison entre TON, TOF et TP

| Caractéristique | TON | TOF | TP |
|---|---|---|---|
| Nom | ON-Delay | OFF-Delay | Pulse |
| Fonction | Retard à l'activation | Retard à la désactivation | Impulsion temporisée |
| Activation de Q | Après PT | Immédiatement lorsque IN devient TRUE | Immédiatement au front montant |
| Désactivation de Q | Lorsque IN devient FALSE | Après PT lorsque IN devient FALSE | À la fin de PT |
| Réinitialisation | IN = FALSE | IN = TRUE annule le retard de désactivation | Dépend du comportement de l'impulsion et des fronts d'entrée |
| Application | Démarrage retardé | Ventilation après arrêt | Signalisation temporaire |

### Astuce pour retenir

- TON : attendre avant d'allumer.
- TOF : attendre avant d'éteindre.
- TP : allumer pendant une durée déterminée.

---

## 5. Les paramètres essentiels d'un temporisateur

Les temporisateurs IEC utilisés dans TIA Portal possèdent des paramètres importants.

### 5.1. IN — Entrée d'activation

IN est l'entrée qui commande le fonctionnement du temporisateur.

Elle reçoit généralement une variable de type BOOL.

Exemples :
- Demande de marche.
- Détection d'une condition.
- Autorisation de fonctionnement.

Valeurs possibles :

- TRUE : entrée active.
- FALSE : entrée inactive.

### 5.2. PT — Temps prédéfini

PT signifie Preset Time.

Il représente le temps de consigne défini par le programmeur.

Exemples :

| Consigne | Signification |
|---|---|
| T#2S | 2 secondes |
| T#5S | 5 secondes |
| T#10S | 10 secondes |
| T#1M | 1 minute |
| T#500MS | 500 millisecondes |

Exemple :

`PT = T#8S`

Le temporisateur possède une consigne de huit secondes.

### 5.3. Q — Sortie du temporisateur

Q est la sortie logique du temporisateur.

Elle indique si la condition de sortie est satisfaite.

Valeurs possibles :

- TRUE : sortie active.
- FALSE : sortie inactive.

Exemple :

Lorsque le TON atteint sa durée de consigne avec IN toujours à TRUE, Q passe à TRUE.

### 5.4. ET — Temps écoulé

ET signifie Elapsed Time.

Il représente le temps réellement écoulé depuis le début de la temporisation, selon le comportement du timer.

Exemple :

- PT = T#8S.
- ET = T#3S.

Cela signifie que trois secondes se sont écoulées sur les huit secondes demandées.

### 5.5. Tableau récapitulatif

| Paramètre | Signification | Type |
|---|---|---|
| IN | Entrée d'activation | BOOL |
| PT | Temps de consigne | TIME |
| Q | Sortie logique | BOOL |
| ET | Temps écoulé | TIME |

**Attention :** PT représente la durée demandée, tandis que ET représente la durée effectivement écoulée.

---

## 6. Applications industrielles

| Application | Temporisateur conseillé | Justification |
|---|---|---|
| Démarrage retardé d'une pompe | TON | Retarde l'activation |
| Ventilation après arrêt d'un moteur | TOF | Retarde la désactivation |
| Signalisation lumineuse pendant 5 secondes | TP | Produit une impulsion |
| Démarrage retardé d'un convoyeur | TON | Introduit un délai avant démarrage |
| Maintien temporaire d'une sortie | TP | Produit une impulsion de durée définie |
| Refroidissement après arrêt d'une machine | TOF | Maintient la ventilation après l'arrêt |

### Exemple : station de pompage

Nous souhaitons réaliser les fonctions suivantes :

1. Démarrer une pompe cinq secondes après une demande continue.
2. Maintenir un ventilateur pendant dix secondes après l'arrêt de la machine.
3. Allumer un voyant pendant trois secondes après une commande.

Choix :

| Fonction | Temporisateur |
|---|---|
| Démarrage retardé de 5 s | TON |
| Ventilation après arrêt pendant 10 s | TOF |
| Voyant pendant 3 s | TP |

---

## 7. Analyse des situations particulières

### Situation 1 : TON interrompu avant la fin

Configuration :

`PT = T#8S`

La demande reste active pendant cinq secondes, puis disparaît.

Résultat immédiatement après la disparition de la demande :

| Paramètre | Valeur |
|---|---|
| IN | FALSE |
| PT | T#8S |
| ET | T#0S |
| Q | FALSE |

Le temporisateur est réinitialisé. Il ne mémorise pas les cinq secondes déjà écoulées.

### Situation 2 : TOF après arrêt

Configuration :

`PT = T#15S`

La machine s'arrête à 14 h 30 min 00 s.

Le ventilateur s'arrête à :

`14 h 30 min 15 s`

La sortie reste active pendant les quinze secondes suivant la désactivation de IN.

### Situation 3 : TP avec bouton maintenu

Configuration :

`PT = T#6S`

L'opérateur maintient le bouton appuyé pendant dix secondes.

Résultat :

- Q passe à TRUE au front montant.
- Q reste active pendant six secondes.
- Q repasse à FALSE après six secondes.
- Le maintien de l'entrée à TRUE ne prolonge pas l'impulsion.

---

## 8. Bonnes pratiques d'utilisation

Lors de la programmation des temporisateurs dans TIA Portal :

1. Choisir le type de temporisateur adapté au besoin.
2. Définir correctement la durée PT.
3. Vérifier les conditions d'activation de IN.
4. Vérifier le comportement de Q.
5. Comprendre les conditions de réinitialisation.
6. Tester les interruptions et les changements d'état.
7. Donner un nom explicite aux temporisateurs.
8. Ajouter des commentaires décrivant leur fonction.

Exemples de noms :

- `TON_Demarrage_Pompe1`
- `TOF_Ventilation_Moteur`
- `TP_Signalisation_Defaut`

Les temporisateurs ne remplacent pas les protections électriques ou les dispositifs de sécurité adaptés à une installation industrielle.

---

## 9. QCM corrigé

**Résultat obtenu : 6/7.**

| N° | Question évaluée | Bonne réponse | Explication |
|---|---|---|---|
| 1 | Démarrer une pompe après 5 secondes | B — TON | Le TON retarde l'activation de Q. |
| 2 | Comportement du TON lorsque IN repasse à FALSE | B | Q repasse à FALSE et ET est réinitialisé. |
| 3 | Ventilation après arrêt d'une machine | B — TOF | Le TOF retarde la désactivation. |
| 4 | Arrêt du ventilateur après 15 secondes | B | Q passe à FALSE après les 15 secondes. |
| 5 | Allumer une lampe pendant 5 secondes | C — TP | Le TP génère une impulsion. |
| 6 | Associer les trois applications aux timers | TON, TOF, TP | Les trois associations sont correctes. |
| 7 | Signification du paramètre PT | B | PT est le temps de consigne. |

**Erreur commise à la question 2 :** le TON ne poursuit pas son comptage lorsque IN repasse à FALSE. Il se réinitialise.

---

## 10. Exercices corrigés

### Exercice 1 — Analyse d'un TON

Une pompe doit démarrer après huit secondes de demande continue.

Configuration :

`PT = T#8S`

L'opérateur maintient la demande pendant cinq secondes, puis relâche le bouton.

Question : quel est l'état du temporisateur immédiatement après le relâchement ?

Réponse donnée : C.

**Bonne réponse : B — Q = FALSE et ET = T#0S.**

Explication :

Le TON nécessite que IN reste à TRUE pendant toute la durée de consigne. Lorsque IN repasse à FALSE, Q est désactivée et ET est réinitialisé.

### Exercice 2 — Analyse d'un TOF

Un ventilateur fonctionne avec un TOF configuré ainsi :

`PT = T#15S`

La machine s'arrête à 14 h 30 min 00 s.

Question : à quel moment le ventilateur s'arrête-t-il ?

**Bonne réponse : C — 14 h 30 min 15 s.**

Explication :

Le TOF maintient sa sortie active pendant quinze secondes après la désactivation de IN.

### Exercice 3 — Analyse d'un TP

Une lampe industrielle est commandée par un TP configuré ainsi :

`PT = T#6S`

L'opérateur appuie sur le bouton et le maintient pendant dix secondes.

Question : que se passe-t-il ?

**Bonne réponse : B — La lampe s'allume pendant six secondes, puis s'éteint automatiquement.**

Explication :

Le TP déclenche une impulsion au front montant. La durée de cette impulsion est déterminée par PT.

### Bilan des exercices

| Exercice | Résultat |
|---|---|
| Exercice 1 — TON | Incorrect |
| Exercice 2 — TOF | Correct |
| Exercice 3 — TP | Correct |
| **Total** | **2/3** |

La notion à renforcer est la réinitialisation du TON lorsque IN repasse à FALSE.

---

## 11. Bilan des travaux pratiques

Les manipulations pratiques ont été déclarées déjà réalisées dans TIA Portal V17.

Elles comprennent :
- La création et la configuration de temporisateurs TON.
- La création et la configuration de temporisateurs TOF.
- La création et la configuration de temporisateurs TP.
- L'utilisation des paramètres IN, PT, Q et ET.

Ces manipulations ne sont donc pas reprises dans cette leçon.

---

## 12. Conclusion générale

Les temporisateurs sont des fonctions essentielles dans la programmation des automates industriels.

Ils permettent de maîtriser le moment d'activation ou de désactivation des équipements et d'organiser des séquences temporelles.

Les trois temporisateurs étudiés possèdent des comportements différents :

- TON : retard à l'enclenchement.
- TOF : retard à la retombée.
- TP : génération d'une impulsion temporisée.

La maîtrise de ces trois fonctions constitue une base indispensable pour programmer des automatismes industriels plus élaborés.

**LEÇON 1 DU MODULE 05 — TERMINÉE ET VALIDÉE**

- QCM : 6/7.
- Exercices : 2/3.
- Travaux pratiques : réalisés.
