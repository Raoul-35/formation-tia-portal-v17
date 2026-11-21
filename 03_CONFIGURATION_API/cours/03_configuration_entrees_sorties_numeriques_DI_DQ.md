# Module 03 – Configuration de l'API S7-1200

# Leçon 3 : Configuration des entrées et sorties numériques (DI/DQ)

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Expliquer le fonctionnement électrique d'une entrée numérique.
- Comprendre les notions de PNP et NPN.
- Identifier les caractéristiques d'une entrée TOR 24 V DC.
- Distinguer les sorties à transistor des sorties à relais.
- Expliquer le raccordement des capteurs et des préactionneurs.
- Identifier les précautions à prendre lors du câblage d'un S7-1200.

# PARTIE I – LES ENTRÉES NUMÉRIQUES (DI)

## 2. Définition d'une entrée numérique

Une entrée numérique, appelée DI (Digital Input), permet à l'automate de recevoir une information provenant d'un équipement extérieur.

Elle possède deux états logiques :

- 0 : état logique bas (FALSE).
- 1 : état logique haut (TRUE).

Exemple : un détecteur de niveau installé sur une cuve.

| État du détecteur | Information logique |
|---|---|
| Niveau non atteint | 0 |
| Niveau atteint | 1 |

Attention : la correspondance entre l'état physique du capteur et le 0 ou 1 dépend du câblage, de la polarité et de la configuration.

## 3. Principe électrique d'une entrée TOR 24 V DC

TOR signifie Tout Ou Rien.

Une entrée TOR 24 V DC détecte un état électrique correspondant aux seuils de tension définis par le constructeur.

Le 24 V DC est une tension très courante dans les circuits de commande industrielle.

Exemple de principe de commande positive :

```text
        ALIMENTATION 24 V DC

             +24 V
                |
                |
         Bouton-poussoir
                |
                |
                +-----------> Entrée DI de l'API

             0 V ------------> Commun de référence

Ce schéma est simplifié. Le raccordement réel doit respecter le type d'entrée, le commun et le schéma constructeur.
PARTIE II – LES CAPTEURS PNP ET NPN
4. Constitution habituelle d'un capteur trois fils
Les capteurs électroniques trois fils utilisent généralement :
Couleur	Fonction habituelle
Marron	Alimentation positive (+24 V)
Bleu	Alimentation négative (0 V)
Noir	Sortie du signal


Les couleurs sont courantes, mais la documentation du fabricant reste la référence.
5. Capteur PNP
Un capteur PNP fournit une tension positive sur sa sortie lorsqu'il est activé.

On dit qu'il commute le potentiel positif.

Fonctionnement général :
- Capteur inactif : sortie généralement proche de 0 V ou non conductrice selon le circuit.
- Capteur actif : sortie proche de +24 V.
Schéma conceptuel :
           +24 V
              |
              |
         CAPTEUR PNP
              |
              | Noir : signal
              |
              +-----------> Entrée DI API

           0 V ------------> Commun DI

Lorsque le capteur détecte un objet, sa sortie transmet généralement le potentiel positif à l'entrée de l'automate.
6. Capteur NPN
Un capteur NPN établit une liaison vers le potentiel négatif lorsqu'il est activé.

On dit qu'il commute le 0 V.

Fonctionnement général :
- Capteur inactif : sortie généralement non conductrice.
- Capteur actif : sortie ramenée vers 0 V.
Le raccordement doit être adapté au type d'entrée et à son commun électrique.
7. Comparaison PNP et NPN
Critère	PNP	NPN
Commutation	Potentiel positif	Potentiel négatif
Sortie active	Proche de +24 V	Proche de 0 V
Logique habituelle	Entrée avec référence 0 V	Entrée avec référence positive
Usage	Très répandu en Europe	Présent dans certaines installations


À retenir :
- PNP : fournit le Plus.
- NPN : commute vers le Négatif.
Un automate peut fonctionner avec des capteurs PNP ou NPN si ses entrées et son câblage sont adaptés.

Il faut vérifier la compatibilité électrique avant tout branchement.
PARTIE III – LES SORTIES NUMÉRIQUES (DQ)
8. Définition d'une sortie numérique
Une sortie numérique, appelée DQ (Digital Output), permet à l'automate de commander un équipement extérieur.

Exemples :
- Contacteur.
- Électrovanne.
- Relais intermédiaire.
- Voyant.
- Avertisseur sonore.
Une sortie numérique possède généralement deux états :
- 0 : sortie inactive.
- 1 : sortie active.
9. Les différents types de sorties numériques
Les deux grandes familles étudiées sont :
1. Sorties à transistor.
2. Sorties à relais.
9.1. Sorties à transistor
Une sortie à transistor est une sortie électronique.

Caractéristiques générales :
- Fonctionnement électronique.
- Commutation rapide.
- Adaptée aux commandes répétitives.
- Généralement utilisée en courant continu.
- Nécessite le respect des limites de courant et de tension.
Sur la CPU S7-1200 1214C DC/DC/DC étudiée, les sorties numériques intégrées sont à transistor.
9.2. Sorties à relais
Une sortie à relais utilise un contact électromécanique.

Caractéristiques générales :
- Contact ouvert ou fermé.
- Possibilité de commuter différents types de circuits dans les limites spécifiées.
- Généralement moins rapide qu'une sortie à transistor.
- Usure mécanique liée aux commutations.
- Respect nécessaire des caractéristiques de charge et de la durée de vie électrique.
9.3. Comparaison transistor et relais
Critère	Transistor	Relais
Technologie	Électronique	Électromécanique
Vitesse de commutation	Rapide	Plus lente
Usure mécanique	Non	Oui
Courant continu	Selon caractéristiques	Selon caractéristiques
Courant alternatif	Selon technologie spécifique	Possible selon caractéristiques
Commutations fréquentes	Adapté	À limiter selon durée de vie


Attention : les sorties à transistor peuvent être de type PNP (commutation positive) ou NPN. Il faut vérifier les caractéristiques exactes du matériel.
10. Commande d'un contacteur par une sortie API
Dans une installation industrielle, l'automate commande généralement un préactionneur, par exemple un contacteur.

Schéma fonctionnel :
          AUTOMATE S7-1200
                 |
                 |
              Sortie DQ
                 |
                 |
          Relais intermédiaire
                 |
                 |
           Bobine contacteur
                 |
                 |
             CONTACTEUR
                 |
                 |
          MOTEUR INDUSTRIEL

Le contacteur assure la commutation du circuit de puissance du moteur.

Le relais intermédiaire peut être utilisé lorsque cela est nécessaire pour adapter l'interface électrique ou protéger la sortie de l'automate.

Le choix dépend des caractéristiques de la charge et des prescriptions du constructeur.
PARTIE IV – PROTECTIONS ET PRÉCAUTIONS ÉLECTRIQUES
11. Protection des charges inductives
Une bobine de relais ou de contacteur est une charge inductive.

Lorsqu'on coupe son alimentation, le champ magnétique s'effondre et provoque une tension induite qui peut être élevée.

Cette surtension peut endommager les composants électroniques de la sortie API.

Pour une bobine en courant continu, une diode de roue libre adaptée peut être installée en parallèle sur la bobine.

Polarité habituelle :
- Cathode côté +24 V.
- Anode côté 0 V.
Pour une bobine en courant alternatif, une protection adaptée, comme un circuit RC ou une varistance, peut être utilisée selon les prescriptions du fabricant.
12. Précautions électriques importantes
Avant de raccorder une entrée ou une sortie numérique, il faut vérifier :
1. La tension nominale du circuit.
2. La polarité de l'alimentation.
3. Le type de sortie du capteur : PNP ou NPN.
4. Le type d'entrée de l'automate.
5. Le courant admissible des sorties.
6. Le courant total admissible du groupe de sorties.
7. La compatibilité de la charge commandée.
8. La protection contre les surtensions inductives.
9. Le raccordement correct des communs électriques.
Règle de sécurité :

Ne jamais effectuer un câblage sur une installation sous tension sans procédure adaptée de consignation et de vérification d'absence de tension.
PARTIE V – ÉVALUATION
13. QCM corrigé
Question 1
Quelle est la fonction principale d'une entrée numérique ?

Réponse : B. Recevoir une information logique provenant d'un équipement extérieur.
Question 2
Quelle est la différence entre un capteur PNP et NPN ?

Réponse : A. Le PNP commute le potentiel positif, tandis que le NPN commute le potentiel négatif.
Question 3
Quelle technologie est utilisée par les sorties intégrées de la CPU 1214C DC/DC/DC ?

Réponse : B. Sorties à transistor.
Question 4
Quelle est la principale caractéristique d'une sortie à relais ?

Réponse : B. Elle utilise un contact électromécanique pour commuter le circuit.
Question 5
Quelle tension fournit généralement la sortie d'un capteur PNP lorsqu'il détecte un objet ?

Réponse : B. +24 V DC.
Question 6
Pourquoi protéger une bobine de relais ou de contacteur en courant continu ?

Réponse : B. Parce qu'elle peut générer une surtension à la coupure du courant.

Résultat du QCM : 6/6.
14. Exercices d'application corrigés
Exercice 1 – Identification d'un capteur
Un détecteur possède les caractéristiques suivantes :
- Marron : +24 V DC.
- Bleu : 0 V.
- Noir : sortie signal.
- Lorsque le détecteur fonctionne, le fil noir fournit une tension proche de +24 V DC.
Réponse : capteur PNP.
Exercice 2 – Technologie de sortie API
La CPU étudiée est une Siemens S7-1200 1214C DC/DC/DC.

Question : quelle technologie de sortie numérique possède cette CPU ?

Réponse : sortie à transistor.

Une bobine de relais intermédiaire 24 V DC peut être commandée si les caractéristiques électriques sont compatibles.
Exercice 3 – Analyse d'un défaut électrique
Un capteur NPN est raccordé à une entrée prévue pour un signal positif provenant d'un capteur PNP.

Le capteur détecte un objet, mais l'automate ne reconnaît pas l'état actif attendu.

Réponse : le capteur NPN commute sa sortie vers 0 V, ce qui ne correspond pas au principe de raccordement prévu.

Un automate peut néanmoins fonctionner avec un capteur NPN si son entrée et son câblage sont adaptés.
Exercice 4 – Protection d'une bobine
Une sortie transistor commande une bobine de relais intermédiaire 24 V DC.

Réponse : une diode de roue libre adaptée peut limiter la surtension à la coupure.

Pour une bobine DC, la cathode est généralement raccordée côté +24 V et l'anode côté 0 V.
Exercice 5 – Analyse d'une application industrielle
Une station de pompage comporte :
- Un capteur de niveau PNP 24 V DC.
- Un bouton-poussoir Marche.
- Un bouton-poussoir Arrêt.
- Un contacteur de pompe avec bobine 24 V DC.
- Un automate S7-1200 CPU 1214C DC/DC/DC.
Réponse :
- Le capteur PNP est raccordé à une entrée DI compatible.
- Les boutons-poussoirs sont raccordés aux entrées DI.
- Le programme API traite les informations.
- Une sortie DQ commande la bobine du contacteur, directement ou par l'intermédiaire d'un relais selon les caractéristiques électriques.
Résultat des exercices : 5/5.
15. Compte rendu de la pratique dans TIA Portal V17
La consultation des paramètres des entrées et sorties numériques a déjà été réalisée dans TIA Portal V17, dans le projet Formation_TIA_Portal_V17.

Matériel étudié :
- Automate : Siemens S7-1200.
- CPU : 1214C DC/DC/DC.
- Firmware : V4.5.
Les manipulations déjà effectuées ont permis de se familiariser avec :
- La configuration matérielle de la CPU.
- La consultation des entrées et sorties intégrées.
- L'identification de la technologie des sorties numériques.
- La distinction entre les entrées numériques et les sorties numériques.
La manipulation pratique n'a pas été répétée.
16. Points essentiels à retenir
- DI signifie entrée numérique.
- DQ signifie sortie numérique.
- TOR signifie Tout Ou Rien.
- PNP commute le potentiel positif.
- NPN commute le potentiel négatif.
- Une sortie à transistor utilise une technologie électronique.
- Une sortie à relais utilise un contact électromécanique.
- La CPU 1214C DC/DC/DC possède des sorties intégrées à transistor.
- Les bobines sont des charges inductives et nécessitent une protection adaptée.
- La compatibilité électrique doit être vérifiée avant tout raccordement.
Fin de la leçon 3 – Module 03
