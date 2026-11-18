# LEÇON 6 — COMPILATION, SAUVEGARDE ET DIAGNOSTIC DANS TIA PORTAL V17

## 1. Objectifs pédagogiques

À la fin de cette leçon, nous devons être capables de :

1. Expliquer le rôle de la compilation dans TIA Portal.
2. Distinguer la compilation matérielle et la compilation logicielle.
3. Identifier les erreurs, avertissements et informations.
4. Comprendre les messages de compilation.
5. Sauvegarder correctement un projet.
6. Utiliser les outils de diagnostic de TIA Portal.
7. Identifier les principales causes d'échec de compilation.

---

## 2. La compilation dans TIA Portal

### 2.1 Définition

La compilation est une opération qui consiste à vérifier la cohérence d'un programme ou d'une configuration matérielle afin de détecter les problèmes avant son utilisation dans l'automate.

Elle permet notamment de vérifier :

- La syntaxe du programme.
- La cohérence des instructions.
- Les déclarations et utilisations des variables.
- La compatibilité de certains types de données.
- La cohérence de la configuration matérielle.

Attention : une compilation réussie ne garantit pas que le programme fonctionnera correctement sur l'installation industrielle. Elle ne valide pas automatiquement la logique du procédé, le câblage physique ou le fonctionnement des équipements.

### 2.2 Compilation matérielle

La compilation matérielle concerne la configuration des équipements présents dans le projet.

Elle peut notamment porter sur :

- L'automate programmable.
- Les modules d'entrées numériques.
- Les modules de sorties numériques.
- Les modules analogiques.
- Les interfaces de communication.
- La configuration des appareils et des réseaux.

Exemple industriel :

Nous configurons un automate S7-1200 avec un module d'entrées analogiques.

La compilation permet de détecter certaines incohérences dans la configuration matérielle.

### 2.3 Compilation logicielle

La compilation logicielle concerne les éléments du programme utilisateur.

Elle porte notamment sur :

- OB (Organization Block).
- FC (Function).
- FB (Function Block).
- DB (Data Block).
- Les variables et les instructions.

Exemple :

Dans notre bloc `FC_Test_SCL`, nous avons écrit :

```scl
#Compteur := 7;

IF #Compteur >= 10 THEN
    #Compteur := 20;
ELSE
    #Compteur := 0;
END_IF;
