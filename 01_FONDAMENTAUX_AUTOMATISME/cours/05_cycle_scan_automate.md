# Leçon 5 — Cycle de fonctionnement (Scan Cycle)

## 1. Définition

Le cycle de fonctionnement, appelé *Scan Cycle*, est le processus répétitif par lequel un automate programmable industriel (API) surveille ses entrées, exécute son programme utilisateur et actualise ses sorties.

Lorsque l'automate est en mode RUN, ce processus se répète continuellement.

## 2. Les principales phases du cycle

### 2.1. Lecture des entrées

L'automate lit l'état de ses entrées physiques et actualise leur représentation dans l'image mémoire des entrées (*Process Image Input*).

Exemple :

| Entrée | Adresse | Fonction |
|---|---|---|
| Bouton Marche | I0.0 | Demande de démarrage |
| Bouton Arrêt | I0.1 | Demande d'arrêt |
| Détecteur de niveau haut | I0.2 | Détection du niveau haut |

Le programme utilisateur utilise ensuite les valeurs mémorisées pour effectuer ses opérations logiques.

### 2.2. Exécution du programme utilisateur

L'automate exécute le programme utilisateur, principalement organisé dans le bloc OB1 pour le fonctionnement cyclique classique du S7-1200.

Il effectue les opérations logiques et détermine les états que doivent prendre les sorties.

Exemple :

- I0.0 = 1 : bouton Marche activé.
- I0.1 = 0 : bouton Arrêt non activé.
- Q0.0 = 1 : demande de commande du contacteur de la pompe.

À cette étape, l'automate calcule les états des sorties dans son image mémoire. Les sorties physiques ne sont pas encore nécessairement actualisées.

### 2.3. Actualisation des sorties

L'automate transfère les états calculés dans l'image mémoire des sorties (*Process Image Output*) vers les sorties physiques concernées.

Exemple :

| Sortie | Adresse | Fonction |
|---|---|---|
| Commande contacteur pompe | Q0.0 | Commande du circuit de puissance |
| Voyant Marche | Q0.1 | Signalisation de fonctionnement |
| Voyant Défaut | Q0.2 | Signalisation d'un défaut |

Lorsque Q0.0 passe à 1, le circuit de commande du contacteur peut être activé si les autres conditions électriques sont satisfaites.

Le contacteur permet ensuite de commander l'alimentation du moteur.

**Remarque :** une sortie automate ne fournit pas nécessairement directement la puissance nécessaire au moteur. Elle commande généralement un préactionneur.

### 2.4. Répétition du cycle

Après ces opérations, l'automate recommence un nouveau cycle.

Tant que la CPU reste en mode RUN, le fonctionnement cyclique se poursuit.

## 3. Schéma simplifié du cycle Scan

```text
       ┌─────────────────────────┐
       │  1. Lecture des entrées │
       │       Image mémoire     │
       └────────────┬────────────┘
                    |
                    v
       ┌─────────────────────────┐
       │  2. Exécution du OB1    │
       │   Programme utilisateur│
       └────────────┬────────────┘
                    |
                    v
       ┌─────────────────────────┐
       │  3. Actualisation des   │
       │     sorties physiques  │
       └────────────┬────────────┘
                    |
                    v
              Cycle suivant
                    |
                    └───────────↻
```

Ce schéma représente le fonctionnement cyclique classique de manière simplifiée.

## 4. Notion de temps de cycle (Cycle Time)

Le temps de cycle est la durée nécessaire à l'automate pour effectuer un cycle de fonctionnement.

Il dépend notamment :

- De la taille du programme utilisateur.
- Du nombre et de la complexité des instructions.
- Des blocs FC et FB appelés.
- Des tâches système et des communications.
- Des interruptions et événements traités par le processeur.

Exemple :

Pour un temps de cycle de 10 ms :

Nombre approximatif de cycles par seconde :

N = 1000 / 10 = 100 cycles/s.

L'automate réalise donc environ 100 cycles par seconde.

Le temps de cycle n'est pas nécessairement constant. Il peut varier selon les opérations exécutées.

## 5. Image mémoire des entrées et des sorties

L'image mémoire des entrées conserve une représentation des états des entrées physiques, utilisée par le programme utilisateur.

L'image mémoire des sorties conserve les états calculés par le programme avant leur transfert vers les sorties physiques.

Exemple :

Un capteur raccordé à I0.0 change d'état juste après la lecture des entrées.

Dans le fonctionnement cyclique classique, cette nouvelle information sera normalement prise en compte lors de la prochaine actualisation de l'image mémoire des entrées.

Cette organisation explique pourquoi une modification physique d'une entrée n'est pas nécessairement traitée instantanément par le programme.

## 6. Le bloc OB1 dans le S7-1200

OB1 (Organization Block 1) est le bloc d'organisation principalement utilisé pour exécuter le programme cyclique utilisateur.

Il faut distinguer :

| Bloc | Fonction |
|---|---|
| OB1 | Exécution cyclique du programme utilisateur |
| OB100 | Exécution au démarrage (Startup) |
| FC | Fonction sans mémoire d'instance propre |
| FB | Bloc fonctionnel avec mémoire d'instance |
| DB | Bloc de données pour stocker des variables |

Le S7-1200 dispose également d'autres blocs d'organisation permettant de traiter des interruptions, des événements matériels et des tâches spécifiques.

## 7. Exemple industriel : station de pompage

Une station de pompage possède :

- Un bouton Marche raccordé à I0.0.
- Un bouton Arrêt raccordé à I0.1.
- Un détecteur de niveau raccordé à I0.2.
- Un contacteur de pompe commandé par Q0.0.

À chaque cycle :

1. L'automate lit les états des boutons et du détecteur.
2. OB1 exécute les conditions de commande.
3. L'automate détermine l'état de Q0.0.
4. Les sorties physiques sont actualisées.
5. Le cycle recommence.

## 8. Points essentiels à retenir

- Le Scan Cycle est le cycle répétitif de fonctionnement de l'automate.
- Le fonctionnement cyclique classique comprend la lecture des entrées, l'exécution du programme et l'actualisation des sorties.
- OB1 est le bloc principal du programme cyclique dans le S7-1200.
- Les images mémoire des entrées et des sorties permettent de gérer les états logiques utilisés par le programme.
- Le temps de cycle représente la durée d'exécution d'un cycle.
- Le temps de cycle peut varier.
- Le S7-1200 peut également traiter des interruptions et d'autres tâches système.

## 9. Conclusion

La compréhension du cycle Scan est indispensable pour programmer et diagnostiquer correctement un automate programmable industriel.

Elle permet notamment de comprendre le comportement des entrées et sorties, les délais de réaction et l'exécution des programmes LAD dans TIA Portal.
