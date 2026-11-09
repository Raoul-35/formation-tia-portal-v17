# Leçon 7 — Modes de fonctionnement d'un automate Siemens S7-1200

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Identifier les différents modes de fonctionnement d'une CPU S7-1200.
- Expliquer les différences entre STOP, STARTUP et RUN.
- Comprendre le rôle de l'OB100 au démarrage.
- Vérifier l'état de fonctionnement d'une CPU dans TIA Portal.
- Comprendre le comportement des sorties lorsque la CPU passe en STOP.
- Distinguer un arrêt logiciel d'une consignation électrique.

## 2. Le mode STOP

Le mode STOP correspond à un état dans lequel la CPU n'exécute pas le programme utilisateur cyclique.

Principales caractéristiques :

- L'OB1 n'est plus exécuté.
- Les opérations cycliques du programme utilisateur sont arrêtées.
- Les sorties prennent les valeurs configurées pour le mode STOP.
- La CPU reste alimentée.
- La communication avec TIA Portal et certaines fonctions de diagnostic restent accessibles.
- Le programme utilisateur reste enregistré dans la mémoire de la CPU.

**Exemple industriel :**

Une pompe est commandée par la sortie Q0.0.

Lorsque la CPU passe en STOP, Q0.0 prend l'état configuré pour ce mode. Si cet état est 0, la commande de la pompe est désactivée.

## 3. Le mode STARTUP

STARTUP est la phase transitoire qui intervient au démarrage de la CPU avant son fonctionnement cyclique normal.

Pendant cette phase :

- La CPU réalise ses opérations de démarrage.
- Les blocs de démarrage configurés peuvent être exécutés.
- Les variables et les données sont préparées selon la configuration et le programme.
- La CPU passe ensuite en RUN si les conditions de démarrage sont satisfaites et que ce mode est demandé.

### Le rôle de l'OB100

L'OB100 est un bloc de démarrage du S7-1200.

Il permet d'exécuter des opérations programmées au démarrage de la CPU.

Exemples :
- Initialiser certaines variables.
- Préparer des états de fonctionnement.
- Réinitialiser certaines données temporaires.
- Préparer une séquence de démarrage.

L'OB100 n'est pas un bloc cyclique comme l'OB1. Il est exécuté lors du démarrage, lorsqu'il est configuré.

## 4. Le mode RUN

Le mode RUN correspond au fonctionnement normal de la CPU.

Dans ce mode :

- Le programme utilisateur cyclique est exécuté.
- L'OB1 est exécuté de manière cyclique.
- Les entrées et sorties sont traitées selon la configuration.
- Les communications et diagnostics restent actifs.
- Les blocs d'interruption configurés peuvent être exécutés lorsque leurs événements se produisent.

**Exemple industriel :**

Dans une station de pompage, le programme surveille le niveau du réservoir, analyse les demandes de fonctionnement et commande les pompes selon les conditions programmées.

## 5. Comparaison des trois modes

| Caractéristique | STOP | STARTUP | RUN |
|---|---|---|---|
| OB1 cyclique | Non | Pas encore | Oui |
| Fonctionnement normal | Non | Phase de démarrage | Oui |
| OB100 | Non, pas d'exécution cyclique | Peut être exécuté | Non, pas cycliquement |
| Diagnostic | Accessible selon les fonctions disponibles | Selon la phase | Accessible |
| Programme utilisateur | Non exécuté cycliquement | Initialisation et démarrage | Exécuté |

## 6. Surveillance de l'état de la CPU dans TIA Portal

Pour vérifier si une CPU est en RUN ou en STOP :

1. Se connecter à la CPU ou à la CPU simulée.
2. Accéder aux fonctions En ligne et diagnostic.
3. Consulter l'état de fonctionnement de la CPU.

Cette vérification est importante pour comprendre pourquoi un programme ne commande pas les sorties attendues.

## 7. Comportement des sorties en STOP

Lorsqu'une CPU passe en STOP, les sorties prennent les valeurs configurées pour cet état.

Il ne faut donc pas supposer qu'elles conservent toujours leur dernière valeur ou qu'elles passent obligatoirement toutes à zéro.

Le comportement dépend de la configuration matérielle et des paramètres de la CPU.

## 8. Sécurité : STOP ne signifie pas consignation

Mettre une CPU en STOP ne garantit pas l'absence de tension dans une installation industrielle.

Les circuits de puissance peuvent rester alimentés. Certains circuits de commande peuvent également être indépendants de l'automate.

Avant toute intervention électrique, une procédure de consignation adaptée doit être appliquée.

Elle comprend notamment :
- La séparation des sources d'énergie.
- La condamnation des dispositifs de séparation.
- L'identification des sources d'énergie.
- La vérification de l'absence de tension (VAT), selon la procédure applicable.

**À retenir : le mode STOP est un état de fonctionnement de l'automate. Ce n'est pas un dispositif de sécurité permettant à lui seul d'intervenir sur une installation.**

## 9. Synthèse

- STOP : la CPU n'exécute plus le programme cyclique utilisateur.
- STARTUP : la CPU réalise ses opérations de démarrage.
- RUN : la CPU exécute normalement le programme utilisateur cyclique.
- OB100 : bloc de démarrage pouvant effectuer des opérations d'initialisation.
- OB1 : bloc principal exécuté cycliquement en RUN.
- L'état de la CPU peut être consulté dans les fonctions En ligne et diagnostic de TIA Portal.
- Le passage en STOP ne remplace jamais une consignation électrique.

## 10. Questions de révision

1. Dans quel mode l'OB1 s'exécute-t-il normalement ?
2. Que se passe-t-il lorsque la CPU passe en STOP ?
3. Quelle est la phase qui précède le fonctionnement cyclique normal ?
4. Quel est le rôle de l'OB100 ?
5. Comment vérifier l'état de fonctionnement d'une CPU dans TIA Portal ?
6. Comment les sorties se comportent-elles en STOP ?
7. Pourquoi le mode STOP ne garantit-il pas la sécurité d'une intervention électrique ?
