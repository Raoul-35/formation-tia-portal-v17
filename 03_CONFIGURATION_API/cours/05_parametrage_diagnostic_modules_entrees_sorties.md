# MODULE 03 — CONFIGURATION DE L'API S7-1200

# LEÇON 5 : PARAMÉTRAGE ET DIAGNOSTIC DES MODULES D'ENTRÉES/SORTIES

## 1. OBJECTIFS PÉDAGOGIQUES

À la fin de cette leçon, nous devons être capables de :

1. Comprendre le paramétrage matériel des modules DI, DQ, AI et AQ.
2. Identifier les paramètres configurables dans TIA Portal V17.
3. Comprendre le fonctionnement du filtrage des entrées numériques.
4. Expliquer le comportement des sorties en cas d'arrêt de la CPU.
5. Comprendre les fonctions de diagnostic des modules analogiques et numériques.
6. Identifier les défauts de câblage, d'alimentation et de configuration.
7. Exploiter les informations de diagnostic pour faciliter les interventions de maintenance industrielle.

---

# PARTIE I — COMPRENDRE LE PARAMÉTRAGE DES MODULES

## 2. DÉFINITION DU PARAMÉTRAGE MATÉRIEL

Le paramétrage matériel consiste à définir les caractéristiques et les conditions de fonctionnement des composants matériels d'un automate programmable industriel.

Lorsqu'un module d'entrées/sorties est installé, il ne suffit pas de le déclarer dans la configuration matérielle de TIA Portal.

Il faut également définir certains paramètres permettant à la CPU et au module de fonctionner correctement.

Ces paramètres dépendent du type de module, de sa référence et de ses caractéristiques techniques.

### 2.1 Exemple industriel

Considérons une station de pompage équipée des éléments suivants :

- Un transmetteur de pression 4–20 mA.
- Un détecteur de niveau TOR.
- Un bouton-poussoir de démarrage.
- Un contacteur de pompe.
- Un variateur de vitesse commandé par un signal 0–10 V.

L'automate doit recevoir et traiter les informations provenant des capteurs, puis commander les actionneurs.

Pour cela, les modules doivent être correctement configurés.

Par exemple, le module analogique recevant le transmetteur de pression doit être paramétré pour mesurer un courant, et non une tension.

### 2.2 Analogie

Imaginons un instrument de mesure installé dans un atelier.

Avant de l'utiliser, nous devons connaître :

- La grandeur qu'il mesure.
- Son unité.
- Sa plage de mesure.
- Son type de signal.
- Ses caractéristiques de fonctionnement.

Un module d'entrées/sorties fonctionne selon un principe comparable.

Il doit connaître les caractéristiques des signaux qu'il reçoit ou qu'il produit.

## 3. PARAMÈTRES SELON LE TYPE DE MODULE

Les paramètres disponibles dépendent de la référence exacte du matériel Siemens utilisé.

| Type de module | Paramètres possibles | Exemple d'utilisation |
|---|---|---|
| DI | Filtrage des entrées, diagnostic de voie selon le module | Bouton-poussoir |
| DQ | Diagnostic de voie, comportement en cas d'arrêt CPU | Commande d'un contacteur |
| AI | Type de mesure, plage, filtrage, diagnostic de rupture de fil selon le module | Transmetteur 4–20 mA |
| AQ | Type de sortie, plage de sortie, diagnostic, comportement en cas d'arrêt CPU selon le module | Commande d'un variateur |

**Important :** tous les modules ne possèdent pas les mêmes paramètres. Il faut consulter les caractéristiques techniques de la référence exacte du module avant de configurer ses propriétés.

---

# PARTIE II — PARAMÉTRAGE DES ENTRÉES NUMÉRIQUES DI

## 4. FILTRAGE DES ENTRÉES NUMÉRIQUES

Une entrée numérique reçoit généralement un signal TOR : 0 ou 1.

Cependant, le signal électrique fourni par un contact mécanique n'est pas toujours parfaitement stable.

### 4.1 Phénomène de rebondissement des contacts

Lorsqu'un opérateur appuie sur un bouton-poussoir mécanique, les contacts électriques peuvent s'ouvrir et se refermer plusieurs fois très rapidement avant de se stabiliser.

Ce phénomène est appelé rebondissement des contacts.

Il peut provoquer plusieurs transitions électriques alors que l'opérateur n'a effectué qu'un seul appui.

### 4.2 Conséquences sur l'automate

Supposons qu'un bouton de démarrage soit raccordé à l'entrée %I0.0.

Pendant l'appui, nous observons les états suivants :

| Temps | État électrique |
|---|---:|
| 0 ms | 0 |
| 1 ms | 1 |
| 2 ms | 0 |
| 3 ms | 1 |
| 4 ms | 0 |
| 5 ms | 1 |

L'automate peut détecter plusieurs transitions si le signal n'est pas correctement filtré.

Cela peut provoquer des déclenchements indésirables dans le programme.

### 4.3 Principe du filtrage temporel

Le filtrage consiste à empêcher la prise en compte de certaines variations trop rapides du signal.

Le module ou le système d'entrées attend que le signal reste stable pendant une durée déterminée avant de valider le changement d'état.

Exemple :

- Temps de filtrage configuré : 8 ms.
- Le signal passe de 0 à 1.
- Le signal revient à 0 après 4 ms.
- La variation n'a pas duré suffisamment longtemps pour être validée.

Le filtrage permet donc de réduire les effets des rebonds de contacts.

### 4.4 Influence du temps de filtrage

| Temps de filtrage | Conséquence |
|---|---|
| Très faible | Risque de laisser passer des perturbations rapides |
| Adapté | Réduction des rebonds et des perturbations |
| Trop élevé | Retard dans la détection d'un véritable changement d'état |

Le temps de filtrage doit être choisi en fonction de l'application.

Pour une commande nécessitant une réaction rapide, un filtrage excessif peut être problématique.

### 4.5 Exemple industriel

Un bouton de démarrage est raccordé à une entrée numérique.

Nous constatons que ses contacts produisent des rebonds pendant environ 4 ms.

Le technicien configure un filtrage de 8 ms.

Le signal parasite disparaît avant la fin du temps de filtrage.

Le changement d'état parasite peut alors être ignoré.

**À retenir :** le filtrage améliore la stabilité des entrées numériques, mais il augmente le temps nécessaire à la validation d'un changement d'état.

## 5. DIAGNOSTIC DES ENTRÉES NUMÉRIQUES

Certains modules numériques disposent de fonctions permettant de détecter des anomalies électriques.

Selon la conception du module, les diagnostics peuvent concerner :

- Les défauts d'alimentation.
- Les courts-circuits.
- Les défauts de voie.
- Certaines ruptures de fil.
- Les anomalies de fonctionnement du module.

La disponibilité de ces fonctions dépend du matériel.

Il ne faut donc pas supposer qu'une entrée numérique standard peut automatiquement détecter une rupture de fil.

---

# PARTIE III — PARAMÉTRAGE DES SORTIES NUMÉRIQUES DQ

## 6. COMPORTEMENT DES SORTIES EN CAS D'ARRÊT DE LA CPU

Une question importante en automatisme concerne le comportement des sorties physiques lorsque la CPU passe en mode STOP.

En mode RUN, le programme utilisateur est exécuté normalement.

En mode STOP, l'exécution normale du programme utilisateur est interrompue.

Il faut donc déterminer ce qu'il advient des sorties physiques.

Selon les possibilités de la CPU, du module et de sa configuration, plusieurs comportements peuvent être prévus :

- Mettre les sorties à 0.
- Appliquer une valeur de remplacement configurée.
- Utiliser un comportement défini par le système.

### 6.1 Exemple industriel

Une sortie %Q0.0 commande le contacteur d'une pompe.

La configuration prévoit une valeur de remplacement égale à 0 en cas d'arrêt de la CPU.

Lorsque la CPU passe en STOP, la sortie prend normalement la valeur logique 0.

Le contacteur peut alors retomber, selon le circuit de commande utilisé.

### 6.2 Tableau de fonctionnement

| Situation | État de la CPU | Comportement |
|---|---|---|
| Demande de marche présente | RUN | Sortie commandée par le programme |
| Demande de marche absente | RUN | Sortie à 0 si le programme le prévoit |
| Arrêt de la CPU | STOP | Comportement selon la configuration matérielle |

### 6.3 Importance pour la sécurité

Le comportement des sorties doit être défini en tenant compte des risques liés au procédé.

Par exemple, l'arrêt d'une pompe peut être souhaitable dans certaines installations, mais dangereux dans d'autres situations.

La sécurité industrielle ne doit pas reposer uniquement sur le programme automate.

Des dispositifs de sécurité indépendants peuvent être nécessaires selon l'analyse des risques.

---

# PARTIE IV — PARAMÉTRAGE DES ENTRÉES ANALOGIQUES AI

## 7. SÉLECTION DU TYPE DE MESURE

Les modules analogiques peuvent accepter différents types de signaux selon leur référence.

Exemples :

- 0–10 V.
- 0–20 mA.
- 4–20 mA.
- Signaux provenant de thermocouples ou de sondes résistives, selon les modules.

Le type de mesure configuré doit correspondre au signal électrique réellement raccordé.

### 7.1 Exemple industriel

Un transmetteur de pression mesure une plage de 0 à 10 bar.

Il fournit un signal de 4–20 mA.

La voie analogique doit être configurée pour mesurer un courant dans la plage appropriée.

Si le module est configuré pour mesurer une tension alors que le transmetteur fournit un courant, la mesure risque d'être incorrecte.

### 7.2 Correspondance entre signal et grandeur physique

| Courant | Pression |
|---|---:|
| 4 mA | 0 bar |
| 8 mA | 2,5 bar |
| 12 mA | 5 bar |
| 16 mA | 7,5 bar |
| 20 mA | 10 bar |

La conversion doit ensuite être réalisée dans le programme automate pour obtenir une valeur exprimée en bar.

## 8. DIAGNOSTIC DE RUPTURE DE FIL

Certains modules analogiques disposent d'une fonction de surveillance permettant de détecter une rupture de fil dans une boucle de mesure.

### 8.1 Fonctionnement d'une boucle 4–20 mA

Dans une boucle de mesure normale :

- 4 mA correspondent à la valeur minimale.
- 20 mA correspondent à la valeur maximale.

Le courant de 4 mA représente le zéro vivant.

Ce principe permet de distinguer une mesure minimale normale d'une absence de signal.

### 8.2 Exemple de rupture de fil

Un transmetteur mesure une pression de 0 à 10 bar.

En fonctionnement normal :

- 4 mA correspondent à 0 bar.
- 12 mA correspondent à 5 bar.
- 20 mA correspondent à 10 bar.

Si un câble est coupé, le courant de la boucle peut tomber à une valeur proche de 0 mA.

Un module compatible avec la surveillance de rupture de fil peut alors signaler une anomalie.

### 8.3 Schéma simplifié

TRANSMETTEUR DE PRESSION
        |
        | Signal 4–20 mA
        |
        v
MODULE ANALOGIQUE AI
        |
        | Surveillance du signal
        |
        v
DIAGNOSTIC AUTOMATE
        |
        +---- Fonctionnement normal
        |
        +---- Défaut détecté

### 8.4 Causes possibles d'un courant nul

Un courant de 0 mA peut notamment être provoqué par :

- Une rupture de câble.
- Une alimentation absente.
- Une borne desserrée.
- Une boucle électrique ouverte.
- Un défaut du transmetteur.

Il ne faut pas interpréter automatiquement 0 mA comme une pression normale de 0 bar.

La détection effective de rupture de fil dépend du module, de son paramétrage et de son raccordement.

## 9. FILTRAGE DES VALEURS ANALOGIQUES

Une mesure analogique peut présenter des variations rapides.

Ces variations peuvent provenir :

- Des perturbations électromagnétiques.
- Des fluctuations du procédé.
- Des vibrations.
- Des parasites électriques.
- Des caractéristiques de l'instrument de mesure.

Le filtrage permet d'atténuer certaines variations rapides afin d'obtenir une mesure plus stable.

### 9.1 Exemple industriel

Un transmetteur mesure une pression normalement proche de 5 bar.

Les mesures reçues sont :

| Échantillon | Pression mesurée |
|---|---:|
| 1 | 5,02 bar |
| 2 | 4,97 bar |
| 3 | 5,08 bar |
| 4 | 4,95 bar |
| 5 | 5,01 bar |

Un filtrage adapté peut réduire les variations rapides.

### 9.2 Attention au filtrage excessif

Un filtrage trop important peut retarder la détection d'une variation réelle.

Par exemple, si la pression augmente rapidement de 5 à 8 bar, un filtrage excessif peut ralentir l'affichage et le traitement de cette variation.

Il faut donc trouver un compromis entre stabilité de la mesure et rapidité de réaction.

---

# PARTIE V — PARAMÉTRAGE DES SORTIES ANALOGIQUES AQ

## 10. CONFIGURATION DE LA PLAGE DE SORTIE

Une sortie analogique permet de produire un signal électrique variable.

Elle peut notamment servir à commander :

- Un variateur de vitesse.
- Une vanne proportionnelle.
- Un régulateur externe.
- Un équipement nécessitant une consigne analogique.

Exemple : un variateur reçoit un signal 0–10 V pour définir sa consigne de vitesse.

### 10.1 Correspondance entre tension et consigne

| Tension de sortie | Consigne |
|---|---:|
| 0 V | 0 % |
| 2,5 V | 25 % |
| 5 V | 50 % |
| 7,5 V | 75 % |
| 10 V | 100 % |

La plage de sortie configurée doit correspondre aux caractéristiques de l'équipement commandé.

Une mauvaise configuration peut provoquer une consigne incorrecte.

## 11. COMPORTEMENT DES SORTIES ANALOGIQUES EN CAS DE DÉFAUT

Lorsqu'une CPU passe en STOP ou qu'un défaut survient, le comportement d'une sortie analogique dépend des capacités du module et de sa configuration.

Des valeurs de remplacement peuvent être prévues sur certains équipements.

### Exemple industriel

Une sortie analogique commande un variateur de vitesse.

Le technicien doit déterminer ce que le module doit produire lorsque la CPU passe en STOP.

Le choix doit être cohérent avec :

- La stratégie de commande.
- Le comportement attendu du variateur.
- Les risques liés au procédé.
- Les exigences de sécurité.

La sortie analogique ne remplace pas les fonctions de sécurité indépendantes.

---

# PARTIE VI — DIAGNOSTIC MATÉRIEL DANS TIA PORTAL V17

## 12. DÉFINITION DU DIAGNOSTIC MATÉRIEL

Le diagnostic matériel permet de surveiller l'état des composants matériels de l'automate et de détecter certaines anomalies.

Il facilite la localisation des défauts et aide le technicien à organiser ses interventions.

Les informations de diagnostic peuvent concerner :

- Une erreur de module.
- Une erreur de configuration.
- Un défaut de voie.
- Un problème d'alimentation.
- Une perte de communication avec un équipement périphérique.
- Une rupture de fil, lorsque le module dispose de cette fonction.

## 13. PRINCIPALES INFORMATIONS DE DIAGNOSTIC

| Information | Signification |
|---|---|
| Module accessible | Le système reconnaît le module |
| Module absent | Le module attendu n'est pas détecté |
| Erreur de configuration | La configuration ne correspond pas au matériel |
| Défaut de voie | Une voie particulière présente une anomalie |
| Défaut d'alimentation | Une alimentation nécessaire est absente ou incorrecte |
| Diagnostic de rupture de fil | Le module compatible a détecté une anomalie de boucle |

### 13.1 Différence entre défaut matériel et défaut de programme

Il est important de distinguer :

**Défaut matériel :**
- Module absent.
- Alimentation défectueuse.
- Câble coupé.
- Défaut électrique d'une voie.

**Défaut de programme :**
- Mauvaise adresse utilisée.
- Mauvaise conversion analogique.
- Mauvaise mise à l'échelle.
- Mauvaise variable utilisée dans le programme.

Un problème d'affichage dans WinCC ne signifie donc pas automatiquement que le module analogique est défectueux.

---

# PARTIE VII — MÉTHODE DE DIAGNOSTIC EN MAINTENANCE INDUSTRIELLE

## 14. CAS PRATIQUE : UN TRANSMETTEUR DE PRESSION NE DONNE PLUS DE MESURE

Une station de pompage possède un transmetteur de pression 4–20 mA.

La valeur affichée dans WinCC reste à 0 bar.

Le technicien doit procéder méthodiquement.

| Étape | Vérification | Objectif |
|---|---|---|
| 1 | Vérifier l'alimentation du transmetteur | Confirmer que l'instrument est alimenté |
| 2 | Vérifier le câblage | Détecter une rupture ou un mauvais raccordement |
| 3 | Mesurer le courant de boucle | Vérifier le signal réellement transmis |
| 4 | Consulter le diagnostic du module AI | Rechercher un défaut matériel |
| 5 | Vérifier la configuration de la voie | Confirmer la plage de mesure |
| 6 | Vérifier l'adressage | Confirmer que le programme lit la bonne entrée |
| 7 | Vérifier la mise à l'échelle | Confirmer la conversion en bar |
| 8 | Vérifier l'affichage WinCC | Contrôler la liaison entre l'automate et l'IHM |

Cette méthode évite de remplacer inutilement un transmetteur alors que le problème peut provenir du câblage, de la configuration ou du programme.

## 15. MÉTHODE GÉNÉRALE DE DIAGNOSTIC

Lorsqu'une entrée ou une sortie ne fonctionne pas correctement, il est préférable de suivre une démarche structurée.

### Étape 1 : Observer le symptôme

Identifier précisément ce qui ne fonctionne pas.

Exemples :
- Une entrée ne change pas d'état.
- Une sortie ne commande pas l'actionneur.
- Une mesure analogique reste à zéro.
- Une valeur affichée dans WinCC est incorrecte.

### Étape 2 : Vérifier l'alimentation

Contrôler les alimentations nécessaires au capteur, au module et aux actionneurs.

### Étape 3 : Vérifier le câblage

Contrôler les bornes, les conducteurs et les raccordements.

### Étape 4 : Vérifier les signaux physiques

Pour une entrée numérique, vérifier la présence du signal électrique.

Pour une entrée analogique, mesurer le courant ou la tension réellement transmis.

### Étape 5 : Consulter le diagnostic matériel

Rechercher les informations de défaut disponibles dans TIA Portal.

### Étape 6 : Vérifier la configuration

Contrôler :
- Le type de module.
- La configuration de la voie.
- Le type de signal.
- Le filtrage.
- Les paramètres de diagnostic.
- Le comportement des sorties.

### Étape 7 : Vérifier le programme automate

Contrôler :
- Les adresses.
- Les variables.
- Les conversions.
- Les conditions logiques.
- Les mises à l'échelle.

### Étape 8 : Vérifier l'IHM

Contrôler les variables utilisées dans WinCC, les unités et les plages d'affichage.

---

# PARTIE VIII — SYNTHÈSE GÉNÉRALE

| Notion | Fonction principale |
|---|---|
| Paramétrage matériel | Définir les caractéristiques de fonctionnement des modules |
| Filtrage DI | Réduire les effets des rebonds et perturbations rapides |
| Diagnostic DI | Détecter certains défauts des entrées numériques |
| Comportement DQ | Définir l'état des sorties dans certaines situations de défaut |
| Type de mesure AI | Adapter le module au signal du capteur |
| Rupture de fil AI | Détecter certaines anomalies de boucle |
| Filtrage AI | Atténuer les variations rapides de mesure |
| Plage AQ | Définir le signal analogique produit |
| Diagnostic matériel | Identifier les anomalies du système |

---

# PARTIE IX — ÉVALUATION THÉORIQUE : QCM

## Question 1 — Filtrage d'une entrée numérique

Dans un système automatisé, quel est le principal objectif du filtrage d'une entrée numérique raccordée à un bouton-poussoir mécanique ?

A. Augmenter la tension du signal électrique.

B. Éliminer les effets des rebonds de contacts et des variations très rapides du signal.

C. Transformer une entrée numérique en entrée analogique.

D. Augmenter la fréquence du cycle automate.

**Réponse correcte : B.**

**Explication :**

Le filtrage d'une entrée numérique permet d'éliminer les effets des rebonds de contacts et des variations très rapides du signal.

Un bouton-poussoir mécanique ne produit pas toujours un signal parfaitement stable au moment de l'appui.

Lorsque ses contacts se ferment, ils peuvent s'ouvrir et se refermer plusieurs fois très rapidement avant de se stabiliser.

L'automate risque alors d'interpréter un seul appui comme plusieurs changements d'état.

Le filtrage permet de ne valider le changement que lorsque le signal reste stable pendant une durée déterminée.

## Question 2 — Rupture de fil

Un transmetteur de pression industriel fournit un signal de 4–20 mA. Le câble de transmission est accidentellement coupé.

Quel défaut un module analogique compatible peut-il détecter si la surveillance correspondante est activée ?

A. Une surtension du réseau électrique.

B. Une erreur de programmation LAD.

C. Une rupture de fil dans la boucle de mesure.

D. Une augmentation automatique de la pression.

**Réponse correcte : C.**

**Explication :**

Dans une boucle de mesure 4–20 mA, le courant circule entre le transmetteur et l'entrée analogique de l'automate.

En fonctionnement normal :
- 4 mA correspondent à la valeur minimale.
- 12 mA correspondent à la valeur intermédiaire.
- 20 mA correspondent à la valeur maximale.

Si le câble est coupé, le courant peut tomber à une valeur proche de 0 mA.

Un module analogique disposant d'une fonction de surveillance de rupture de fil peut alors signaler ce défaut.

Cette détection dépend du module, de son paramétrage et de son raccordement.

## Question 3 — Comportement des sorties en STOP

Dans TIA Portal, pourquoi faut-il configurer correctement le comportement des sorties numériques lorsqu'une CPU passe en mode STOP ?

A. Pour augmenter la vitesse du processeur.

B. Pour déterminer l'état des sorties dans cette situation.

C. Pour modifier automatiquement les adresses des sorties.

D. Pour transformer les sorties numériques en sorties analogiques.

**Réponse correcte : B.**

**Explication :**

Lorsqu'une CPU S7-1200 passe du mode RUN au mode STOP, l'exécution normale du programme automate s'arrête.

Il faut donc définir le comportement des sorties physiques.

Selon les possibilités du matériel et la configuration, une sortie peut notamment :
- Passer à 0.
- Prendre une valeur de remplacement configurée.
- Appliquer un comportement défini par le système.

Le choix doit tenir compte des risques liés au procédé.

La sécurité d'une installation ne doit pas reposer uniquement sur le programme automate.

## Question 4 — Configuration d'une entrée analogique

Dans une installation industrielle, un transmetteur de pression fournit un signal de 4–20 mA. Pourtant, dans TIA Portal, la voie analogique est configurée pour mesurer une tension de 0–10 V.

Quelle est la conséquence probable ?

A. La mesure sera correctement interprétée automatiquement.

B. La CPU augmentera automatiquement le courant du transmetteur.

C. La mesure risque d'être incorrecte parce que le type de signal configuré ne correspond pas au signal réel.

D. Le module analogique se transformera automatiquement en sortie analogique.

**Réponse correcte : C.**

**Explication :**

Le module analogique doit être configuré en fonction du signal électrique réellement fourni par le transmetteur.

Dans cet exemple :
- Signal réel : 4–20 mA.
- Configuration TIA Portal : 0–10 V.

Il existe donc une incompatibilité entre le signal physique et le paramétrage matériel.

Le module risque de fournir une mesure incorrecte ou de signaler un défaut.

Le paramétrage matériel doit toujours correspondre aux caractéristiques électriques du capteur ou du transmetteur.

## Question 5 — Filtrage analogique

Dans une installation industrielle, quel est le principal intérêt du filtrage d'une entrée analogique qui mesure la pression ?

A. Augmenter artificiellement la pression mesurée.

B. Réduire les variations rapides et les perturbations de la mesure.

C. Transformer le signal analogique en signal TOR.

D. Modifier automatiquement la plage de mesure du transmetteur.

**Réponse correcte : B.**

**Explication :**

Le filtrage analogique permet de stabiliser une mesure affectée par des fluctuations rapides.

Exemple de mesures :
- 5,02 bar.
- 4,97 bar.
- 5,08 bar.
- 4,95 bar.
- 5,01 bar.

Ces petites variations peuvent provenir de perturbations électriques ou de fluctuations du procédé.

Le filtrage atténue ces variations pour obtenir une valeur plus stable.

Un filtrage trop important peut cependant ralentir la détection d'une variation réelle de pression.

## Question 6 — Diagnostic matériel

Dans TIA Portal V17, quel est l'intérêt principal du diagnostic matériel ?

A. Modifier automatiquement le programme LAD.

B. Détecter et identifier certaines anomalies affectant les composants matériels de l'automate.

C. Augmenter la mémoire de la CPU.

D. Remplacer automatiquement un module défectueux.

**Réponse correcte : B.**

**Explication :**

Le diagnostic matériel permet au technicien de surveiller l'état des équipements configurés dans TIA Portal.

Il peut notamment aider à identifier :
- Un module absent ou inaccessible.
- Une erreur de configuration matérielle.
- Un défaut de voie.
- Un problème d'alimentation.
- Une rupture de fil, si le module dispose de cette fonction.

Le diagnostic ne répare pas automatiquement le défaut. Il fournit des informations pour faciliter sa localisation.

## Question 7 — Mise à l'échelle

Un transmetteur de pression mesure une plage de 0 à 10 bar et fournit un signal de 4–20 mA.

Après vérification, le technicien constate que le courant de boucle est correct, mais que WinCC affiche une pression incorrecte.

Quelle vérification doit-il privilégier ?

A. Remplacer immédiatement le transmetteur.

B. Vérifier la mise à l'échelle de la valeur analogique dans le programme automate.

C. Modifier la fréquence du réseau électrique.

D. Remplacer la CPU sans effectuer d'autres contrôles.

**Réponse correcte : B.**

**Explication :**

Le transmetteur fournit un courant correct. Le problème peut donc se situer dans le traitement de la valeur analogique par l'automate.

Il faut vérifier :
1. La valeur brute reçue par l'entrée analogique.
2. La normalisation avec NORM_X.
3. La mise à l'échelle avec SCALE_X.
4. La variable transmise à WinCC.
5. La configuration de l'affichage dans WinCC.

Exemple :

Pour un transmetteur de 0 à 10 bar :

Pression = ((Courant - 4) / (20 - 4)) × 10

Pour 12 mA :

Pression = ((12 - 4) / 16) × 10

Pression = 5 bar.

Si WinCC affiche 10 bar, il faut rechercher une erreur dans la chaîne de traitement ou d'affichage.

### Résultat du QCM

7 bonnes réponses sur 7.

Score : 100 %.

---

# PARTIE X — EXERCICES D'APPLICATION CORRIGÉS

## Exercice 1 — Filtrage d'une entrée numérique

Un bouton-poussoir de démarrage est raccordé à une entrée DI d'un automate S7-1200.

Le technicien constate que le bouton produit des rebonds électriques pendant environ 4 ms.

Il configure un temps de filtrage de 8 ms.

Que se passe-t-il si le signal parasite disparaît après 4 ms, avant que le temps de filtrage soit écoulé ?

A. L'automate valide obligatoirement l'appui.

B. L'automate peut ignorer cette variation puisqu'elle n'a pas duré suffisamment longtemps.

C. L'automate transforme automatiquement le signal en analogique.

D. La CPU passe automatiquement en STOP.

**Réponse correcte : B.**

### Correction détaillée

Le temps de filtrage configuré est de 8 ms.

Le signal parasite ne dure que 4 ms.

Comparaison :

Durée du signal parasite = 4 ms.

Temps de filtrage = 8 ms.

Comme 4 ms est inférieur à 8 ms, le signal parasite peut être ignoré.

Le changement d'état ne reste pas stable suffisamment longtemps pour être validé.

**À retenir :** le filtrage permet de réduire les effets des rebonds de contacts. Un filtrage trop faible peut laisser passer des perturbations, tandis qu'un filtrage trop élevé peut retarder une véritable commande.

## Exercice 2 — Diagnostic d'une boucle analogique

Un transmetteur de pression 4–20 mA mesure une pression de 0 à 16 bar.

Pendant le fonctionnement, le technicien mesure un courant de 0 mA dans la boucle.

Quel défaut doit-il suspecter en priorité ?

A. Une pression exactement égale à 0 bar, sans autre anomalie possible.

B. Une rupture de fil, une perte d'alimentation ou une ouverture de la boucle de courant.

C. Une pression supérieure à 16 bar.

D. Une mauvaise configuration de WinCC uniquement.

**Réponse correcte : B.**

### Correction détaillée

Dans une boucle 4–20 mA :

| Courant | Pression |
|---|---:|
| 4 mA | 0 bar |
| 8 mA | 4 bar |
| 12 mA | 8 bar |
| 16 mA | 12 bar |
| 20 mA | 16 bar |

Un courant de 0 mA n'est pas une mesure normale de 0 bar.

En effet, 0 bar correspond à 4 mA, et non à 0 mA.

Un courant nul peut notamment indiquer :
- Un câble coupé.
- Une alimentation du transmetteur absente.
- Une borne desserrée.
- Une boucle électrique ouverte.

### Démarche de maintenance

Avant de remplacer le transmetteur, le technicien doit :

1. Vérifier son alimentation.
2. Contrôler le câblage.
3. Vérifier les connexions.
4. Mesurer le courant de boucle.
5. Consulter le diagnostic du module analogique.

**Notion importante : le zéro vivant.**

Le courant de 4 mA représente le zéro de mesure. Cela permet de distinguer une mesure minimale normale d'une absence de signal.

## Exercice 3 — Configuration d'une entrée analogique

Dans une station de pompage, un transmetteur de pression de 0 à 12 bar fournit un signal de 4–20 mA.

Le courant mesuré est de 16 mA.

Quelle pression doit afficher l'automate si la mise à l'échelle est correctement réalisée ?

A. 6 bar.

B. 8 bar.

C. 9 bar.

D. 12 bar.

**Réponse correcte : C.**

### Correction détaillée

Données :

- Pression minimale : 0 bar.
- Pression maximale : 12 bar.
- Courant minimal : 4 mA.
- Courant maximal : 20 mA.
- Courant mesuré : 16 mA.

**Étape 1 : calculer la proportion du signal.**

Proportion = (16 - 4) / (20 - 4)

Proportion = 12 / 16

Proportion = 0,75.

**Étape 2 : convertir en pression.**

Pression = 0 + (0,75 × 12)

Pression = 9 bar.

### Vérification

| Courant | Pression |
|---|---:|
| 4 mA | 0 bar |
| 8 mA | 3 bar |
| 12 mA | 6 bar |
| 16 mA | 9 bar |
| 20 mA | 12 bar |

### Lien avec TIA Portal

Dans le programme automate, nous pouvons utiliser :

- NORM_X pour normaliser la valeur brute.
- SCALE_X pour convertir la valeur normalisée en pression exprimée en bar.

## Exercice 4 — Diagnostic d'une sortie numérique

Une sortie numérique %Q0.0 commande le contacteur d'une pompe.

Le technicien constate que la CPU est passée en mode STOP.

Dans la configuration matérielle, le comportement de la sortie en cas de STOP est défini sur une valeur de remplacement égale à 0.

Que doit-il normalement se passer ?

A. La sortie reste obligatoirement à 1.

B. La sortie prend la valeur de remplacement 0.

C. La sortie devient une entrée analogique.

D. La CPU redémarre automatiquement en RUN.

**Réponse correcte : B.**

### Correction détaillée

La sortie %Q0.0 commande le contacteur de la pompe.

Lorsque la CPU passe en mode STOP, le programme utilisateur n'est plus exécuté normalement.

Dans notre exercice :

- Sortie concernée : %Q0.0.
- Mode CPU : STOP.
- Valeur de remplacement configurée : 0.

La sortie prend donc normalement l'état logique 0.

Le contacteur peut retomber lorsque la sortie passe à 0, selon le circuit de commande.

**Attention :** le comportement électrique réel dépend aussi du type de sortie et du câblage. Il faut également vérifier les exigences de sécurité de l'installation.

## Exercice 5 — Diagnostic complet d'une chaîne analogique

Dans une installation industrielle, un transmetteur de pression 0–10 bar fournit un signal 4–20 mA à un module AI.

Le technicien effectue les vérifications suivantes :

- Alimentation du transmetteur : correcte.
- Courant de boucle : 12 mA.
- Configuration du module AI : correcte.
- Valeur affichée dans WinCC : 10 bar.

Pourtant, la pression réelle correspond à 5 bar.

Quelle vérification doit être effectuée en priorité ?

A. Remplacer le transmetteur de pression.

B. Vérifier la normalisation et la mise à l'échelle dans le programme automate.

C. Remplacer immédiatement le module AI.

D. Augmenter la tension d'alimentation du transmetteur.

**Réponse correcte : B.**

### Correction détaillée

Données :

- Plage de pression : 0–10 bar.
- Signal du transmetteur : 4–20 mA.
- Courant mesuré : 12 mA.
- Pression réelle : 5 bar.
- Pression affichée dans WinCC : 10 bar.

**Étape 1 : vérifier la conversion.**

Pression = ((12 - 4) / (20 - 4)) × 10

Pression = (8 / 16) × 10

Pression = 5 bar.

**Étape 2 : localiser l'origine probable du problème.**

Le courant de boucle est correct et la configuration du module AI est correcte.

Il faut donc examiner la chaîne de traitement :

TRANSMETTEUR 4–20 mA
        |
        v
MODULE ANALOGIQUE AI
        |
        v
VALEUR BRUTE
        |
        v
NORM_X
        |
        v
SCALE_X
        |
        v
VARIABLE AUTOMATE (bar)
        |
        v
WINCC
        |
        v
AFFICHAGE : 10 bar

Une erreur de paramétrage de NORM_X, de SCALE_X ou de la variable utilisée dans WinCC pourrait expliquer l'affichage incorrect.

### Résultat des exercices

| Exercice | Résultat |
|---|---|
| 1. Filtrage d'une entrée DI | Incorrect |
| 2. Diagnostic d'une boucle 4–20 mA | Correct |
| 3. Mise à l'échelle analogique | Correct |
| 4. Comportement d'une sortie en STOP | Correct |
| 5. Diagnostic d'une chaîne analogique | Correct |

Résultat : 4/5, soit 80 %.

La notion à revoir est le fonctionnement du filtrage temporel des entrées numériques.

---

# PARTIE XI — COMPTE RENDU DE LA PRATIQUE TIA PORTAL V17

## 16. PRATIQUE MATÉRIELLE DÉJÀ RÉALISÉE

L'exploration pratique des propriétés des modules d'entrées/sorties a déjà été effectuée dans TIA Portal V17.

Les manipulations ne sont donc pas à refaire.

Les éléments étudiés comprennent :

| Élément exploré | Objectif |
|---|---|
| Propriétés des entrées numériques DI | Identifier les paramètres disponibles |
| Paramètres de filtrage | Comprendre le traitement des changements d'état |
| Paramètres de diagnostic | Identifier les possibilités de surveillance |
| Propriétés des sorties numériques DQ | Examiner les paramètres de fonctionnement |
| Comportement des sorties en STOP | Comprendre les valeurs de remplacement |
| Propriétés des modules analogiques | Examiner les paramètres des voies AI/AQ |

### 16.1 Compétences pratiques acquises

Cette exploration permet de mieux comprendre :

- L'organisation des propriétés matérielles dans TIA Portal.
- Les paramètres associés aux différents types de modules.
- L'importance de la cohérence entre le câblage et la configuration.
- Le rôle du diagnostic matériel dans la maintenance.
- L'influence du paramétrage sur le fonctionnement des entrées et sorties.

### 16.2 Observation importante

Les possibilités de configuration varient selon les références des modules.

Une fonction présentée dans un cours général ne doit pas être considérée comme disponible sur tous les modules S7-1200.

Il faut toujours vérifier les paramètres réellement proposés pour le matériel sélectionné.

---

# PARTIE XII — CONCLUSION GÉNÉRALE

Le paramétrage et le diagnostic des modules d'entrées/sorties constituent des compétences essentielles pour la mise en service et la maintenance des systèmes automatisés.

Le technicien doit savoir configurer les modules en fonction des caractéristiques des capteurs et des actionneurs.

Il doit également comprendre le rôle du filtrage, les comportements des sorties en cas de défaut et les possibilités de diagnostic matériel.

Dans une installation industrielle, une panne apparente d'un capteur peut provenir de plusieurs éléments :

- L'instrument lui-même.
- Son alimentation.
- Le câblage.
- Le module d'entrées/sorties.
- La configuration matérielle.
- Le programme automate.
- L'affichage dans WinCC.

Une démarche de diagnostic structurée permet de localiser le défaut sans remplacer inutilement des composants.

**Compétence essentielle à retenir :** savoir distinguer un défaut de mesure, un défaut de câblage, un défaut matériel et une erreur de traitement dans le programme automate.

---

# FIN DE LA LEÇON 5
## MODULE 03 — CONFIGURATION DE L'API S7-1200
