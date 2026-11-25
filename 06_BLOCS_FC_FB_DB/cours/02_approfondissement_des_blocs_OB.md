# MODULE 06 — BLOCS FC, FB ET DB
## LEÇON 2 — APPROFONDISSEMENT DES BLOCS D'ORGANISATION (OB)

### 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Expliquer le rôle des blocs d'organisation (OB).
- Distinguer les principaux types d'OB.
- Comprendre le fonctionnement des OB de démarrage et d'interruption cyclique.
- Comprendre le principe des interruptions matérielles.
- Identifier les OB de gestion des erreurs et des événements de diagnostic.
- Créer et configurer des OB dans TIA Portal V17.

---

### 2. Rappel : qu'est-ce qu'un OB ?

OB signifie **Organization Block**, ou bloc d'organisation.

Un OB constitue un point d'entrée dans le programme de l'automate. Il est exécuté lorsque le système d'exploitation de la CPU détecte l'événement auquel cet OB est associé.

Contrairement à un FC ou à un FB, un OB n'est généralement pas appelé directement par une instruction d'appel dans OB1. Son exécution est déclenchée par le système d'exploitation de la CPU.

### 3. Les principaux types d'OB

| Type d'OB | Fonction | Déclenchement |
|---|---|---|
| OB1 | Programme cyclique principal | Cycles normaux de la CPU |
| Startup OB | Initialisation au démarrage | Passage de STOP à RUN |
| Cyclic Interrupt OB | Exécution périodique | À intervalles configurés |
| Hardware Interrupt OB | Réaction à un événement matériel | Événement matériel configuré |
| Time-delay Interrupt OB | Exécution après un délai déclenché | Événement avec délai configuré |
| OB80 | Gestion de certaines erreurs temporelles | Erreur de temps d'exécution |
| OB82 | Gestion de certains événements de diagnostic | Événement de diagnostic matériel |

**Attention :** les OB disponibles et leurs conditions d'utilisation dépendent de la CPU, de son firmware et de sa configuration matérielle.

---

### 4. OB1 : le programme cyclique principal

OB1 est le bloc principal du programme utilisateur.

Il est exécuté de manière cyclique par la CPU.

Principe simplifié :

1. Lecture des entrées selon la configuration de la CPU.
2. Exécution du programme utilisateur.
3. Mise à jour des sorties selon le fonctionnement configuré.
4. Passage au cycle suivant.

Les accès directs aux entrées et sorties peuvent avoir un comportement différent de celui de l'image processus.

---

### 5. Startup OB : bloc de démarrage

Le Startup OB est exécuté lors du passage de la CPU de STOP à RUN.

Il peut servir à :

- Initialiser certaines variables.
- Réinitialiser des états de séquence lorsque cela est prévu.
- Préparer les données nécessaires au fonctionnement.
- Définir des valeurs initiales de fonctionnement.

#### Exemple pratique réalisé

Nous avons créé dans TIA Portal un bloc :

`OB_DEMARRAGE`

Type : Startup OB  
Langage : LAD.

Nous avons également créé les variables suivantes :

| Variable | Type | Fonction |
|---|---|---|
| Alarme_Active | BOOL | État d'une alarme |
| Sequence_Active | BOOL | État d'une séquence |
| Demarrage_Initialise | BOOL | Indication de l'initialisation |

Programmation réalisée :

- Réseau 1 : remise à zéro de `Alarme_Active` avec une bobine R.
- Réseau 2 : remise à zéro de `Sequence_Active` avec une bobine R.
- Réseau 3 : mise à 1 de `Demarrage_Initialise` avec une bobine S.

La compilation du bloc a été réalisée avec succès.

**Précaution industrielle :** la remise à zéro d'une alarme ne doit jamais supprimer une condition de sécurité réelle ni masquer un défaut encore présent. L'initialisation doit respecter la philosophie de sécurité de l'installation.

---

### 6. Cyclic Interrupt OB : interruption cyclique

Un Cyclic Interrupt OB est exécuté automatiquement à intervalles réguliers.

Exemple :

Un OB configuré avec une période de 100 ms est déclenché théoriquement toutes les 100 millisecondes.

Applications industrielles :

- Acquisition périodique de mesures.
- Traitement régulier de certaines grandeurs.
- Calculs périodiques.
- Surveillance de paramètres nécessitant un intervalle déterminé.

#### 6.1. Relation entre période et fréquence

La fréquence correspond au nombre d'exécutions par seconde.

Formule :

**f = 1000 / T**

Avec :
- f : fréquence en hertz (Hz).
- T : période en millisecondes (ms).

| Période | Fréquence |
|---:|---:|
| 50 ms | 20 Hz |
| 100 ms | 10 Hz |
| 200 ms | 5 Hz |
| 500 ms | 2 Hz |
| 1 000 ms | 1 Hz |

Plus la période est courte, plus l'OB est exécuté fréquemment.

#### 6.2. Exemple industriel

Une installation doit effectuer une lecture de température toutes les 500 ms.

La période à configurer est donc :

T = 500 ms

La fréquence correspondante est :

f = 1000 / 500 = 2 Hz

L'OB est ainsi déclenché deux fois par seconde.

#### 6.3. Travaux pratiques réalisés

Nous avons créé :

`OB_MESURE_CYCLIQUE`

Type : Cyclic Interrupt OB  
Langage : LAD  
Période configurée : 100 ms.

Nous avons créé une variable :

`Compteur_Mesures`

Type : INT.

Nous avons ensuite préparé un réseau utilisant le bloc ADD :

| Paramètre | Valeur |
|---|---|
| IN1 | Compteur_Mesures |
| IN2 | 1 |
| OUT | Compteur_Mesures |

Principe :

`Compteur_Mesures := Compteur_Mesures + 1`

À chaque déclenchement de l'OB, le compteur augmente d'une unité.

À 100 ms, il augmente théoriquement de 10 unités par seconde.

**Limite importante :** une variable INT signée peut atteindre au maximum 32 767. En partant de zéro, le compteur atteint cette limite après environ 54 minutes et 36 secondes à 10 incréments par seconde. Dans une application réelle, il faut prévoir une remise à zéro, une limitation ou un type de données plus adapté.

#### 6.4. Attention au temps d'exécution

La période configurée ne doit pas être confondue avec le temps nécessaire à l'exécution du programme.

Par exemple, un OB configuré à 100 ms doit effectuer son traitement dans des conditions compatibles avec cette périodicité.

Un traitement trop long peut entraîner des retards ou des erreurs temporelles.

---

### 7. Hardware Interrupt OB : interruption matérielle

Un Hardware Interrupt OB est déclenché lorsqu'un événement matériel configuré se produit.

Exemple :

Une entrée numérique est configurée pour détecter un front montant.

Lorsqu'un événement correspondant se produit, la CPU déclenche l'OB associé.

Applications :

- Détection rapide d'un événement provenant d'un capteur.
- Comptage d'événements matériels.
- Réaction à certains signaux externes.

La disponibilité de cette fonction dépend des caractéristiques de la CPU et du module utilisé, ainsi que de leur configuration.

Il ne faut pas confondre une interruption matérielle avec une simple lecture cyclique d'une entrée dans OB1.

---

### 8. Time-delay Interrupt OB : interruption temporisée

Ce type d'OB permet de déclencher une exécution après un délai associé à un événement.

Il est différent d'un TON :

- Le TON est une instruction de temporisation utilisée dans le programme.
- Le Time-delay Interrupt OB est un bloc d'organisation déclenché par le système d'exploitation après un délai configuré ou demandé.

Son utilisation dépend des fonctions disponibles sur la CPU.

---

### 9. OB80 : erreur temporelle

OB80 est associé à certaines erreurs temporelles.

Exemple :

Une situation dans laquelle le temps d'exécution du programme dépasse une limite configurée peut provoquer un événement temporel.

Selon la CPU, le firmware et la configuration, cet événement peut entraîner l'exécution d'OB80.

Ce bloc peut permettre de traiter ou de diagnostiquer certains événements temporels.

Il ne remplace pas l'optimisation du programme et ne garantit pas à lui seul que la CPU continuera à fonctionner.

---

### 10. OB82 : interruption de diagnostic

OB82 est associé à certains événements de diagnostic matériel.

Exemples possibles :

- Défaut détecté sur un module.
- Événement de diagnostic provenant d'un équipement.
- Modification de l'état de diagnostic d'un matériel compatible.

La disponibilité et les événements pris en charge dépendent du matériel et de la configuration.

---

### 11. Priorité des OB

Les OB ne sont pas tous exécutés dans les mêmes conditions de priorité.

Certains événements peuvent interrompre l'exécution d'OB1 ou d'un autre traitement de priorité inférieure.

Après le traitement de l'interruption, l'exécution interrompue peut reprendre.

Le comportement exact dépend des classes de priorité et des règles de la CPU.

---

### 12. Différence entre les OB et les temporisateurs

| OB cyclique | Temporisateur TON |
|---|---|
| Déclenché périodiquement | Mesure un délai |
| Fonctionnement associé au système d'exploitation | Fonction utilisée dans le programme |
| Peut exécuter un traitement toutes les 100 ms | Peut activer une sortie après 5 secondes |
| Sert notamment à l'acquisition périodique | Sert notamment aux temporisations de commande |

---

### 13. QCM corrigé

**Question 1 :** Quel est le rôle principal d'OB1 ?

Réponse : C — Exécuter le programme cyclique principal.

**Question 2 :** Quand un Startup OB est-il exécuté ?

Réponse : B — Lors du passage de STOP à RUN.

**Question 3 :** Quel OB est adapté à une exécution toutes les 100 ms ?

Réponse : C — Cyclic Interrupt OB.

**Question 4 :** Quel OB réagit à un événement matériel configuré ?

Réponse : B — Hardware Interrupt OB.

**Question 5 :** Quel OB est associé à certaines erreurs temporelles ?

Réponse : B — OB80.

**Question 6 :** Quel OB est associé à certains événements de diagnostic ?

Réponse : C — OB82.

**Question 7 :** Que peut-il se passer lorsqu'un OB de priorité supérieure interrompt OB1 ?

Réponse : B — L'OB prioritaire s'exécute, puis OB1 peut reprendre son exécution.

---

### 14. Exercices corrigés

**Exercice 1 :**

Un OB cyclique possède une période de 200 ms.

Fréquence :

f = 1000 / 200 = 5 Hz.

Réponse : 5 exécutions par seconde.

**Exercice 2 :**

Nous souhaitons 20 exécutions par seconde.

Période :

T = 1000 / 20 = 50 ms.

Réponse : 50 ms.

**Exercice 3 :**

Nous souhaitons effectuer une mesure toutes les 500 ms.

Réponse : période de 500 ms, soit 2 exécutions par seconde.

---

### 15. Bilan des travaux pratiques

| Travail | État |
|---|---|
| Création de OB_DEMARRAGE | Réalisée |
| Déclaration des variables d'initialisation | Réalisée |
| Programmation des réseaux LAD du Startup OB | Réalisée |
| Compilation de OB_DEMARRAGE | Réussie |
| Création de OB_MESURE_CYCLIQUE | Réalisée |
| Configuration de la période à 100 ms | Réalisée |
| Déclaration de Compteur_Mesures | Réalisée |
| Préparation du réseau ADD | Réalisée |
| Compilation du réseau cyclique | Confirmée |
| Validation en simulation avec PLCSIM | Non réalisée dans cette séance |

La simulation n'a pas été validée en raison d'un blocage de la machine virtuelle Windows 10 au lancement de PLCSIM. Les paramètres de VirtualBox ont été ajustés : 16 Go de RAM, 4 processeurs et accélération 3D désactivée. TIA Portal s'ouvre normalement. La validation avec PLCSIM reste à effectuer ultérieurement.

---

### 16. Conclusion

Les blocs d'organisation constituent les points d'entrée du programme utilisateur déclenchés par le système d'exploitation de la CPU.

Nous avons approfondi leur fonctionnement, créé un Startup OB et configuré un OB d'interruption cyclique dans TIA Portal V17.

Cette connaissance est indispensable pour structurer des programmes industriels fiables et adaptés aux événements du procédé.

**Fin de la leçon 2 — Module 06.**
