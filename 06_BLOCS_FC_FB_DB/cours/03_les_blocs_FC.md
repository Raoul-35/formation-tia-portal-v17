# MODULE 06 — BLOCS FC, FB ET DB
## LEÇON 3 : LES BLOCS FC (FUNCTION)

### 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

- Comprendre le rôle d'un bloc FC.
- Distinguer un FC d'un FB.
- Identifier les différentes interfaces d'un FC.
- Créer un FC en LAD, FBD ou SCL.
- Appeler un FC depuis OB1.
- Transmettre des paramètres à un FC et récupérer ses résultats.
- Réutiliser un même FC pour plusieurs équipements industriels.

---

### 2. Définition d'un FC

FC signifie Function (fonction).

Un FC est un bloc de programmation permettant de regrouper un traitement logique, mathématique ou technique afin de pouvoir le réutiliser dans différentes parties d'un programme automate.

Exemples :
- Conversion d'une mesure analogique.
- Calcul d'une moyenne.
- Addition de plusieurs valeurs.
- Mise à l'échelle d'un capteur.
- Vérification de limites de fonctionnement.

Un FC ne possède pas de DB d'instance propre.

Il peut néanmoins accéder aux données globales contenues dans les DB globaux.

### 3. Différence entre FC et FB

| Caractéristique | FC | FB |
|---|---|---|
| Signification | Function | Function Block |
| DB d'instance | Non nécessaire | Nécessaire |
| Mémoire STAT propre | Non | Oui |
| Variables TEMP | Oui | Oui |
| Paramètres IN | Oui | Oui |
| Paramètres OUT | Oui | Oui |
| Paramètres IN_OUT | Oui | Oui |
| Réutilisation | Oui | Oui |
| Conservation d'état interne | Non, pas de mémoire d'instance | Oui, avec STAT |
| Utilisation | Traitements et calculs | Gestion d'équipements et séquences avec mémoire |

**Remarque :** un FC peut utiliser des DB globaux et modifier leurs données. L'absence de DB d'instance ne signifie donc pas qu'il ne peut pas travailler avec des données mémorisées ailleurs.

---

### 4. Les interfaces d'un FC

Les paramètres d'interface déterminent les échanges de données entre le bloc appelant et le FC.

#### 4.1 IN — Entrée

Reçoit une donnée transmise par le bloc appelant.

Exemple : une valeur brute provenant d'un capteur analogique.

#### 4.2 OUT — Sortie

Transmet un résultat calculé par le FC au bloc appelant.

Exemple : une pression calculée en bars.

#### 4.3 IN_OUT — Entrée/Sortie

Permet au FC de recevoir une variable existante, de la modifier et de restituer sa nouvelle valeur.

Exemple : corriger une consigne de température en lui ajoutant un décalage.

#### 4.4 TEMP — Variable temporaire

Stocke des résultats intermédiaires pendant l'exécution du FC.

Attention :
- Une variable TEMP n'est pas destinée à conserver une valeur entre deux appels.
- Elle doit recevoir une valeur avant d'être utilisée.
- Elle ne doit pas être considérée comme une mémoire permanente.

---

### 5. Exemple 1 : FC_CONVERSION

Nous avons précédemment créé un bloc nommé `FC_CONVERSION`.

Son rôle est de convertir une mesure analogique brute en pourcentage.

#### Interface

| Section | Variable | Type |
|---|---|---|
| Input | Valeur_Brute | INT |
| Output | Niveau_Pourcent | REAL |
| Temp | Calcul | REAL |

#### Code SCL

```scl
Calcul := INT_TO_REAL(Valeur_Brute) / 27648.0 * 100.0;
Niveau_Pourcent := Calcul;
```

#### Explication

1. La valeur brute INT est convertie en REAL.
2. Le calcul est effectué dans la variable temporaire `Calcul`.
3. Le résultat est transmis à `Niveau_Pourcent`.

Exemple :

Pour une valeur brute de 13824 :

Niveau_Pourcent = 50 %

Cette formule suppose une plage normalisée de 0 à 27648 correspondant à 0 à 100 %.

---

### 6. Exemple 2 : Création de FC_ADDITION

Nous avons créé un deuxième FC dans TIA Portal V17.

#### Paramètres

| Section | Variable | Type |
|---|---|---|
| Input | Valeur_1 | REAL |
| Input | Valeur_2 | REAL |
| Output | Somme | REAL |

#### Code SCL

```scl
Somme := Valeur_1 + Valeur_2;
```

#### Exemple de fonctionnement

Valeur_1 = 12.5

Valeur_2 = 7.5

Somme = 20.0

Le FC reçoit deux nombres et renvoie leur somme.

---

### 7. Appel d'un FC depuis OB1

Un FC doit être appelé depuis un autre bloc pour être exécuté.

Dans notre exercice, nous avons appelé `FC_ADDITION` depuis OB1.

Architecture :

```text
             OB1
              |
              |
        Appel FC_ADDITION
              |
       +------+------+
       |             |
   Valeur_1       Valeur_2
     12.5           7.5
       |             |
       +------+------+
              |
          ADDITION
              |
           Somme
              |
              v
       DB_RESULTATS
              |
       Somme = 20.0
```

Nous avons utilisé un DB global nommé `DB_RESULTATS`.

Variable déclarée :

| Nom | Type | Valeur initiale |
|---|---|---|
| Somme | REAL | 0.0 |

La sortie du FC est associée à :

`"DB_RESULTATS".Somme`

**Résultat attendu :** 20.0.

La compilation a été réussie. Toutefois, l'exécution réelle n'a pas été validée avec PLCSIM.

---

### 8. Applications industrielles des FC

| Nom du FC | Fonction | Application |
|---|---|---|
| FC_CONVERSION | Conversion analogique | Niveau d'une cuve |
| FC_ADDITION | Addition | Calcul de mesures |
| FC_MOYENNE | Moyenne | Températures |
| FC_ECHELLE | Mise à l'échelle | Pression |
| FC_DIAGNOSTIC | Analyse de données | Détection de valeurs anormales |
| FC_CONVERSION_PRESSION | Conversion | Capteurs de pression |

#### Exemple : installation de quatre pompes

Nous disposons de quatre capteurs de pression.

Au lieu de créer quatre FC différents, nous créons un seul bloc `FC_CONVERSION_PRESSION` et nous l'appelons quatre fois avec des paramètres différents.

Formule :

P = (Valeur brute / 27648) × 10

Pour une plage de 0 à 10 bars :

| Capteur | Valeur brute | Pression |
|---|---:|---:|
| 1 | 13824 | 5 bar |
| 2 | 6912 | 2,5 bar |
| 3 | 20736 | 7,5 bar |
| 4 | 27648 | 10 bar |

Cette méthode réduit les répétitions et facilite la maintenance du programme.

---

### 9. Bonnes pratiques de programmation

1. Donner des noms explicites aux FC.
2. Utiliser les paramètres IN pour les données reçues.
3. Utiliser OUT pour transmettre les résultats.
4. Réserver IN_OUT aux variables qui doivent être modifiées.
5. Initialiser les variables TEMP avant leur utilisation.
6. Éviter les répétitions de code.
7. Documenter les calculs et les unités physiques.
8. Vérifier les limites des valeurs reçues.
9. Compiler les blocs après modification.
10. Tester les FC dans des conditions représentatives de l'installation.

---

### 10. Évaluation des connaissances

**Question 1 :** Quelle interface permet de recevoir et modifier une variable existante ?

Réponse : IN_OUT.

**Question 2 :** Quelle interface sert aux calculs intermédiaires ?

Réponse : TEMP.

**Question 3 :** Pour additionner deux nombres et renvoyer le résultat, quelles interfaces utiliser ?

Réponse : Deux IN et un OUT.

**Question 4 :** Quelle est la différence entre OUT et IN_OUT ?

Réponse : OUT transmet un résultat, tandis que IN_OUT permet de modifier une variable reçue.

**Question 5 :** Quel est l'intérêt principal d'un FC ?

Réponse : Réutiliser un traitement sans devoir réécrire les mêmes instructions.

**Question 6 :** Peut-on appeler un même FC plusieurs fois ?

Réponse : Oui, avec des paramètres différents.

---

### 11. Travaux pratiques réalisés

| Travail pratique | Résultat |
|---|---|
| Création de FC_ADDITION | Réalisée |
| Déclaration des paramètres IN | Réalisée |
| Déclaration du paramètre OUT | Réalisée |
| Programmation en SCL | Réalisée |
| Compilation du FC | Réussie |
| Appel depuis OB1 | Réalisé |
| Création de DB_RESULTATS | Réalisée |
| Association de la sortie au DB | Réalisée |
| Compilation du programme | Réussie |
| Vérification avec PLCSIM | Non réalisée |

**Bilan :** les opérations de création, de programmation et de compilation ont été réalisées. Le test d'exécution reste à effectuer ultérieurement.

---

### 12. Conclusion

Les blocs FC sont essentiels pour structurer les programmes industriels.

Ils permettent de séparer les traitements, de réutiliser les fonctions et de simplifier la maintenance des applications automatisées.

Ils sont particulièrement adaptés aux calculs, conversions, traitements mathématiques et fonctions de diagnostic.

La prochaine leçon sera consacrée à l'approfondissement des blocs FB (Function Block), qui permettent notamment de gérer la mémoire interne des équipements grâce aux variables STAT et aux DB d'instance.
