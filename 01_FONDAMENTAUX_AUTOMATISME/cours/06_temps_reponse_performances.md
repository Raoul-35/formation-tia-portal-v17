# Leçon 6 — Temps de réponse et performances d'un automate

## 1. Objectifs de la leçon

À la fin de cette leçon, nous devons être capables de :

- Définir le temps de cycle et le temps de réponse d'un automate.
- Distinguer ces deux notions.
- Identifier les éléments qui influencent le temps de réponse.
- Comprendre les conséquences d'un cycle trop long.
- Expliquer le fonctionnement des compteurs rapides (HSC).
- Comprendre le principe des interruptions matérielles.
- Expliquer le rôle du filtrage des entrées.

## 2. Temps de cycle et temps de réponse

### 2.1. Temps de cycle (Cycle Time)

Le temps de cycle est la durée nécessaire à l'automate pour effectuer un cycle de fonctionnement.

Il comprend notamment les opérations de lecture des entrées, d'exécution du programme utilisateur et d'actualisation des sorties, ainsi que certaines opérations système.

Exemple :

Un automate possède un temps de cycle de 10 ms.

Nombre approximatif de cycles par seconde :

N = 1000 / 10 = 100 cycles/s.

L'automate réalise donc environ 100 cycles par seconde.

### 2.2. Temps de réponse (Response Time)

Le temps de réponse représente la durée qui s'écoule entre l'apparition d'un événement physique et la réaction effective du système automatisé.

Exemple : un capteur détecte le niveau haut d'un réservoir et l'automate doit arrêter une pompe.

La chaîne de réaction comprend :

1. Détection du niveau par le capteur.
2. Transmission du signal électrique.
3. Prise en compte de l'information par l'automate.
4. Exécution du programme utilisateur.
5. Actualisation de la sortie automate.
6. Réaction du contacteur.
7. Arrêt de l'alimentation du moteur.

Le temps total de cette réaction constitue le temps de réponse global du système.

## 3. Éléments influençant le temps de réponse

| Élément | Explication |
|---|---|
| Temps du capteur | Temps nécessaire pour détecter l'événement et produire le signal |
| Temps du module d'entrée | Temps de traitement électrique et électronique |
| Temps de prise en compte API | Délai lié à la lecture cyclique des entrées |
| Temps d'exécution du programme | Durée nécessaire au traitement logique |
| Temps du module de sortie | Délai de commutation de la sortie |
| Temps du préactionneur | Temps de réaction du relais ou du contacteur |
| Temps de l'actionneur | Temps nécessaire à la réaction physique de l'équipement |

Le temps de réponse réel dépend également de la synchronisation entre l'événement et le cycle de l'automate.

## 4. Exemple numérique

Considérons les valeurs pédagogiques suivantes :

| Élément | Temps |
|---|---:|
| Détection du capteur | 2 ms |
| Traitement de l'entrée | 3 ms |
| Prise en compte et traitement API | 10 ms |
| Commutation de la sortie | 2 ms |
| Réaction du contacteur | 15 ms |
| Total simplifié | 32 ms |

Calcul :

T = 2 + 3 + 10 + 2 + 15

T = 32 ms.

Le système peut donc nécessiter environ 32 ms entre le début de la détection et la réaction du contacteur, selon les hypothèses retenues.

Dans une installation réelle, il faut tenir compte des caractéristiques des équipements, de la synchronisation des opérations et des éventuelles variations du temps de cycle.

## 5. Détection des événements rapides

Un problème apparaît lorsqu'un signal est plus court que le temps séparant deux lectures cycliques des entrées.

Exemple :

- Temps de cycle de l'automate : 10 ms.
- Durée d'une impulsion du capteur : 2 ms.

Si cette impulsion apparaît et disparaît entre deux lectures des entrées, l'automate risque de ne jamais la détecter.

Applications concernées :

- Comptage de bouteilles sur un convoyeur.
- Détection de pièces sur une machine rapide.
- Mesure de vitesse d'un moteur.
- Comptage des impulsions d'un codeur incrémental.

## 6. Compteur rapide (HSC)

HSC signifie High-Speed Counter.

Le compteur rapide permet de compter des impulsions à une fréquence supérieure à ce que permettrait une simple lecture cyclique classique.

Exemple : un codeur incrémental installé sur un moteur produit des impulsions lorsque son arbre tourne.

Le HSC permet de compter ces impulsions à l'aide d'une fonction matérielle spécialisée de la CPU.

| Élément | Fonction |
|---|---|
| Codeur incrémental | Produit des impulsions selon le déplacement ou la rotation |
| HSC | Compte les impulsions rapides |
| CPU | Exploite les valeurs de comptage dans le programme |

Les possibilités et limites du HSC dépendent de la référence exacte de la CPU et de sa configuration.

## 7. Interruptions matérielles

Une interruption matérielle permet de déclencher un traitement particulier à la suite d'un événement matériel configuré, sans attendre le prochain cycle cyclique normal d'OB1.

Sur le S7-1200, un bloc d'organisation d'interruption matérielle peut être configuré pour traiter certains événements sur des entrées compatibles.

Exemple :

1. Un détecteur détecte le passage d'une pièce.
2. L'événement matériel configuré déclenche une interruption.
3. La CPU exécute le traitement prévu dans l'OB correspondant.
4. L'exécution cyclique normale reprend.

Une interruption ne provoque pas automatiquement une action mécanique. Le programme doit définir le traitement à effectuer.

## 8. Différence entre HSC et interruption matérielle

| Critère | HSC | Interruption matérielle |
|---|---|---|
| Signification | High-Speed Counter | Hardware Interrupt |
| Fonction principale | Compter des impulsions rapides | Déclencher un traitement sur événement |
| Exemple | Comptage de bouteilles | Détection d'une pièce |
| Fonctionnement | Comptage matériel spécialisé | Exécution d'un OB d'interruption |

Ces deux mécanismes peuvent être utilisés conjointement dans une même installation.

## 9. Filtrage des entrées

Dans une installation industrielle, un signal électrique peut présenter des changements parasites provoqués par :

- Les vibrations mécaniques.
- Les perturbations électromagnétiques.
- Les rebonds des contacts électriques.
- Les instabilités du signal.

Le filtrage permet de limiter la prise en compte de ces perturbations.

### Exemple : rebond d'un bouton poussoir

Lorsqu'un opérateur appuie sur un bouton mécanique, le contact peut produire plusieurs changements d'état très rapides avant de se stabiliser.

Ce phénomène est appelé rebond de contact.

Un filtrage adapté permet d'éviter que ces changements parasites soient interprétés comme plusieurs commandes distinctes.

Attention : un filtrage trop important peut ralentir la détection d'événements rapides.

## 10. Schéma récapitulatif

```text
Événement physique
       |
       v
     CAPTEUR
       |
       v
Entrée de l'automate
       |
       +-----------------------+
       |                       |
       v                       v
Lecture cyclique          Interruption
       |                   matérielle
       v                       |
     OB1                 OB d'interruption
       |                       |
       +-----------+-----------+
                   |
                   v
          Traitement logique
                   |
                   v
          Actualisation sortie
                   |
                   v
             PRÉACTIONNEUR
                   |
                   v
               ACTIONNEUR
```

Ce schéma est pédagogique et simplifié. Le traitement des interruptions et l'actualisation des sorties dépendent de la configuration de la CPU.

## 11. Points essentiels à retenir

- Le temps de cycle est la durée d'exécution d'un cycle de l'automate.
- Le temps de réponse est la durée entre un événement physique et la réaction effective du système.
- Le temps de réponse dépend de plusieurs composants de la chaîne automatisée.
- Une impulsion trop courte peut ne pas être détectée par une lecture cyclique classique.
- Le HSC permet de compter des impulsions rapides.
- Une interruption matérielle déclenche un traitement particulier sur événement configuré.
- Le filtrage permet de limiter les perturbations des signaux d'entrée.
- Un filtrage trop important peut ralentir la détection.

## 12. Conclusion

La maîtrise du temps de réponse est indispensable pour concevoir des automatismes industriels fiables.

Elle permet de choisir les capteurs, les modules d'entrées et les mécanismes de traitement adaptés aux contraintes de vitesse de l'installation.

Dans la suite de la formation, ces notions seront mises en pratique avec TIA Portal V17 et le simulateur S7-PLCSIM.
