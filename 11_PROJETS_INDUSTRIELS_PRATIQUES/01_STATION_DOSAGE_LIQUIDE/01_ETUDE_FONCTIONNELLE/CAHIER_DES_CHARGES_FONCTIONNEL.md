# CAHIER DES CHARGES FONCTIONNEL (CdCF)

## PROJET 01 — STATION INDUSTRIELLE DE DOSAGE DE LIQUIDE

**Environnement :** Siemens TIA Portal V17  
**Automate cible :** S7-1200 CPU 1214C  
**Simulation API :** S7-PLCSIM  
**Supervision :** WinCC Runtime Advanced  
**Version du document :** 1.0  
**Statut :** Document de conception — base pédagogique

---

# 1. PRÉSENTATION DU PROJET

## 1.1. Contexte

Le projet consiste à concevoir une station industrielle automatisée permettant de doser et de mélanger deux liquides selon des recettes définies par un opérateur.

L'installation devra assurer le transfert des liquides, le contrôle des quantités, le mélange, la vidange ainsi que la surveillance des équipements.

Le système sera développé avec Siemens TIA Portal V17, simulé avec S7-PLCSIM et supervisé avec WinCC Runtime Advanced.

## 1.2. Objectif général

Développer une application automatisée capable de :

- Doser précisément deux liquides.
- Respecter les paramètres d'une recette.
- Mélanger les produits pendant une durée définie.
- Évacuer le mélange obtenu.
- Gérer les modes de fonctionnement.
- Détecter et traiter les défauts.
- Afficher les informations du procédé sur une interface de supervision.

## 1.3. Objectifs spécifiques

Le système devra permettre :

1. La sélection et la validation des recettes.
2. Le contrôle des conditions initiales.
3. Le dosage automatique du liquide A.
4. Le dosage automatique du liquide B.
5. Le contrôle du niveau de la cuve.
6. La gestion du mélange.
7. La gestion de la vidange.
8. La surveillance des équipements.
9. La gestion des alarmes.
10. L'enregistrement des données de production.

# 2. PÉRIMÈTRE DU SYSTÈME

## 2.1. Fonctions incluses

Le système prendra en charge :

- Le contrôle des deux réservoirs sources.
- Le transfert des liquides A et B.
- La mesure des débits et des volumes transférés.
- Le contrôle du niveau de la cuve de mélange.
- La commande de l'agitateur.
- La commande de la vanne de vidange.
- La gestion des recettes.
- Les modes automatique, manuel et arrêt.
- La gestion des défauts et des alarmes.
- La supervision avec WinCC.

## 2.2. Fonctions exclues

La première version du projet ne prendra pas en charge :

- Le contrôle de la qualité chimique du produit.
- Le dosage de plus de deux liquides.
- La gestion d'une chaîne d'emballage.
- La gestion complète d'un magasin de matières premières.
- Le pilotage d'une installation physique.
- La certification d'une fonction de sécurité.

Ces fonctions pourront faire l'objet d'évolutions ultérieures.

# 3. DESCRIPTION DE L'INSTALLATION

## 3.1. Équipements principaux

| Repère | Désignation | Fonction |
|---|---|---|
| TK-101 | Réservoir A | Stockage du liquide A |
| TK-102 | Réservoir B | Stockage du liquide B |
| TK-103 | Cuve de mélange | Réception et mélange des liquides |
| P-101 | Pompe A | Transfert du liquide A |
| P-102 | Pompe B | Transfert du liquide B |
| FT-101 | Débitmètre A | Mesure du débit A |
| FT-102 | Débitmètre B | Mesure du débit B |
| LT-103 | Transmetteur de niveau | Mesure du niveau de TK-103 |
| AG-101 | Agitateur | Homogénéisation du mélange |
| XV-103 | Vanne de vidange | Évacuation du produit fini |

## 3.2. Principe de fonctionnement

Le liquide A est transféré depuis TK-101 vers TK-103 par P-101.

Le liquide B est transféré depuis TK-102 vers TK-103 par P-102.

Les débitmètres FT-101 et FT-102 permettent de mesurer les volumes transférés.

Après les deux dosages, AG-101 assure le mélange des produits.

À la fin du mélange, XV-103 permet la vidange de la cuve.

# 4. FONCTIONS DE SERVICE

## 4.1. Fonction principale

**FP-01 :** préparer automatiquement un mélange liquide à partir de deux produits, conformément à une recette sélectionnée et validée.

## 4.2. Fonctions complémentaires

| Référence | Fonction |
|---|---|
| FC-01 | Mesurer les quantités de liquides transférées. |
| FC-02 | Surveiller le niveau de la cuve. |
| FC-03 | Commander les pompes de transfert. |
| FC-04 | Commander l'agitateur. |
| FC-05 | Commander la vanne de vidange. |
| FC-06 | Gérer les recettes. |
| FC-07 | Gérer les modes de fonctionnement. |
| FC-08 | Détecter les défauts du procédé. |
| FC-09 | Gérer les alarmes. |
| FC-10 | Superviser l'installation. |
| FC-11 | Enregistrer les informations de production. |
| FC-12 | Gérer les interruptions et les reprises. |

# 5. EXIGENCES FONCTIONNELLES

Les exigences fonctionnelles décrivent les comportements que le système doit obligatoirement assurer.

| Référence | Exigence | Priorité |
|---|---|---|
| EF-001 | Permettre la sélection d'une recette. | Haute |
| EF-002 | Vérifier la validité des paramètres de recette. | Haute |
| EF-003 | Vérifier les conditions initiales avant démarrage. | Haute |
| EF-004 | Autoriser le démarrage du cycle automatique sur demande opérateur. | Haute |
| EF-005 | Commander P-101 pour transférer le liquide A. | Haute |
| EF-006 | Mesurer le volume A transféré. | Haute |
| EF-007 | Arrêter P-101 lorsque la quantité demandée est atteinte. | Haute |
| EF-008 | Commander P-102 pour transférer le liquide B. | Haute |
| EF-009 | Mesurer le volume B transféré. | Haute |
| EF-010 | Arrêter P-102 lorsque la quantité demandée est atteinte. | Haute |
| EF-011 | Autoriser le mélange après confirmation des deux dosages. | Haute |
| EF-012 | Respecter le temps de mélange défini dans la recette. | Haute |
| EF-013 | Arrêter l'agitateur à la fin du mélange. | Haute |
| EF-014 | Commander la vanne de vidange. | Haute |
| EF-015 | Surveiller le niveau pendant la vidange. | Haute |
| EF-016 | Confirmer la fin du cycle. | Haute |
| EF-017 | Proposer un mode manuel sécurisé. | Haute |
| EF-018 | Détecter les défauts des équipements. | Haute |
| EF-019 | Détecter les anomalies des mesures. | Haute |
| EF-020 | Signaler les alarmes à l'opérateur. | Haute |
| EF-021 | Permettre le réarmement contrôlé des défauts. | Haute |
| EF-022 | Afficher l'état du procédé sur WinCC. | Haute |
| EF-023 | Enregistrer les données de production. | Moyenne |
| EF-024 | Conserver les informations utiles à l'analyse d'une interruption. | Haute |

# 6. EXIGENCES DE PERFORMANCE

Les valeurs suivantes constituent les hypothèses de référence pour la simulation.

## 6.1. Paramètres de capacité et de fonctionnement

| Paramètre | Valeur retenue |
|---|---:|
| Capacité de TK-103 | 200 L |
| Débit maximal FT-101 | 100 L/min |
| Débit maximal FT-102 | 100 L/min |
| Volume A, recette 01 | 100 L |
| Volume B, recette 01 | 50 L |
| Temps de mélange | 120 s |
| Niveau bas | 10 % |
| Niveau bas de vidange | 5 % |
| Niveau haut | 85 % |
| Niveau très haut | 95 % |

## 6.2. Précision de dosage

Pour les besoins pédagogiques, nous retenons une tolérance de dosage de ±1 %.

Exemple pour une consigne de 100 L :

- Limite inférieure : 99 L.
- Consigne : 100 L.
- Limite supérieure : 101 L.

Cette tolérance constitue un objectif de simulation et non une garantie de précision industrielle.

## 6.3. Temps de réponse

Les temporisations de détection des défauts et les temps maximaux de fonctionnement seront définis lors de la conception détaillée.

Ils devront être suffisamment réalistes pour permettre des essais pertinents.

# 7. EXIGENCES DE SÉCURITÉ ET INTERVERROUILLAGES

| Référence | Exigence | Réaction attendue |
|---|---|---|
| SEC-001 | Prendre en compte l'arrêt d'urgence. | Mise en sécurité des équipements concernés. |
| SEC-002 | Empêcher le fonctionnement à sec des pompes. | Interdiction de marche si le réservoir source est insuffisant. |
| SEC-003 | Protéger la cuve contre le débordement. | Arrêt des transferts en cas de niveau très haut. |
| SEC-004 | Interdire l'utilisation d'une mesure invalide. | Arrêt de l'opération dépendante. |
| SEC-005 | Surveiller les défauts moteurs. | Arrêt de l'équipement concerné et signalisation. |
| SEC-006 | Éviter les redémarrages automatiques après défaut. | Réarmement et nouvelle autorisation nécessaires. |
| SEC-007 | Maintenir les protections en mode manuel. | Refus des commandes non autorisées. |
| SEC-008 | Empêcher la vidange pendant le remplissage. | Interverrouillage des commandes concernées. |

**Note de sécurité :** l'arrêt d'urgence d'une installation réelle nécessite une architecture de sécurité adaptée, indépendante des simples commandes standard de l'automate. Notre simulation ne constitue pas une validation de sécurité industrielle.

# 8. EXIGENCES DE GESTION DES DÉFAUTS

| Référence | Défaut | Réaction attendue |
|---|---|---|
| DEF-001 | Défaut pompe A | Arrêt du dosage A et alarme. |
| DEF-002 | Défaut pompe B | Arrêt du dosage B et alarme. |
| DEF-003 | Absence de débit A | Arrêt du transfert A. |
| DEF-004 | Absence de débit B | Arrêt du transfert B. |
| DEF-005 | Niveau très haut | Arrêt des transferts. |
| DEF-006 | Défaut agitateur | Interruption du mélange. |
| DEF-007 | Défaut transmetteur de niveau | Blocage des opérations dépendantes de la mesure. |
| DEF-008 | Temps de dosage dépassé | Arrêt du dosage concerné. |
| DEF-009 | Défaut vanne | Interruption ou blocage de la vidange. |
| DEF-010 | Arrêt d'urgence | Déclenchement de la fonction de sécurité. |

Chaque défaut devra posséder :
- Une condition d'apparition.
- Une réaction automatique.
- Une indication sur WinCC.
- Une condition de disparition.
- Une procédure de réarmement si nécessaire.

# 9. EXIGENCES DE GESTION DES RECETTES

Le système devra permettre la sélection de plusieurs recettes.

Exemple de recettes pédagogiques :

| Paramètre | RECETTE_01 | RECETTE_02 | RECETTE_03 |
|---|---:|---:|---:|
| Volume A | 100 L | 80 L | 120 L |
| Volume B | 50 L | 70 L | 30 L |
| Temps mélange | 120 s | 150 s | 180 s |
| Volume total | 150 L | 150 L | 150 L |

Avant chaque démarrage, les contrôles suivants seront effectués :

1. Volumes strictement positifs.
2. Volume total compatible avec la capacité de la cuve.
3. Niveau initial compatible avec la recette.
4. Quantités disponibles dans les réservoirs sources.
5. Temps de mélange dans les limites autorisées.

Une recette invalide doit être refusée.

# 10. EXIGENCES DE SUPERVISION WINCC

La supervision devra comporter les vues suivantes :

| Vue | Fonction |
|---|---|
| VUE_01_SYNOPTIQUE | Représentation générale du procédé. |
| VUE_02_PRODUCTION | Suivi du cycle et de l'étape active. |
| VUE_03_RECETTES | Sélection et paramétrage des recettes. |
| VUE_04_COMMANDES | Commandes opérateur autorisées. |
| VUE_05_EQUIPEMENTS | État des pompes, de l'agitateur et de la vanne. |
| VUE_06_ALARMES | Alarmes actives et historique. |
| VUE_07_TENDANCES | Courbes de débit et de niveau. |
| VUE_08_STATISTIQUES | Compteurs et données de production. |

# 11. EXIGENCES DE PROGRAMMATION MODULAIRE

Le programme devra respecter les principes suivants :

- Chaque équipement doit disposer d'une logique de commande clairement identifiée.
- Les FB seront utilisés pour les équipements et fonctions nécessitant une mémorisation d'état.
- Les FC seront utilisés pour les calculs et traitements sans mémoire propre.
- Les UDT permettront de structurer les données.
- Les DB d'instance seront associés aux FB.
- Les DB globaux seront utilisés pour les recettes, paramètres et données communes.
- OB1 assurera principalement l'organisation et l'appel des blocs.

L'architecture détaillée sera élaborée avant la programmation.

# 12. CRITÈRES DE VALIDATION ET PLAN D'ESSAIS

Chaque exigence importante devra être vérifiée par un essai.

| Référence | Scénario d'essai | Résultat attendu |
|---|---|---|
| TEST-001 | Démarrage avec conditions valides | Cycle autorisé. |
| TEST-002 | Arrêt d'urgence actif | Démarrage interdit. |
| TEST-003 | Réservoir A vide | Pompe A interdite. |
| TEST-004 | Dosage A à 100 L | Arrêt à la consigne. |
| TEST-005 | Dosage B à 50 L | Arrêt à la consigne. |
| TEST-006 | Défaut pompe A | Cycle interrompu. |
| TEST-007 | Perte du signal FT-101 | Dosage A arrêté. |
| TEST-008 | Niveau très haut | Transferts interrompus. |
| TEST-009 | Fin du temps de mélange | Agitateur arrêté. |
| TEST-010 | Fin de vidange | Vanne refermée. |
| TEST-011 | Recette invalide | Démarrage refusé. |
| TEST-012 | Défaut pendant un dosage | Reprise contrôlée après réarmement. |
| TEST-013 | Sélection du mode manuel | Commandes autorisées selon les interverrouillages. |
| TEST-014 | Fin d'un cycle complet | Compteur de production incrémenté. |

Les résultats réels de ces essais seront consignés dans un document distinct pendant la phase de validation.

# 13. CONTRAINTES TECHNIQUES

## 13.1. Matériel et logiciel

Le projet sera réalisé avec :

- TIA Portal V17.
- S7-1200 CPU 1214C.
- S7-PLCSIM.
- WinCC Runtime Advanced.
- Un environnement Windows virtualisé.

## 13.2. Contraintes de simulation

Les signaux de terrain seront simulés :
- Débits.
- Niveaux.
- Retours moteurs.
- Défauts équipements.
- Positions de vanne.

La communication entre l'automate simulé et WinCC devra être configurée et testée avant les essais de supervision.

## 13.3. Limites

La simulation permettra de vérifier la logique de commande, les séquences, les alarmes et les échanges avec la supervision.

Elle ne permettra pas de valider :
- Les performances hydrauliques réelles.
- Les caractéristiques physiques des pompes.
- La précision réelle des instruments.
- La sécurité fonctionnelle d'une installation physique.

# 14. DOCUMENTS LIVRABLES

À la fin du projet, les documents suivants devront être disponibles :

| N° | Document |
|---|---|
| 1 | Analyse fonctionnelle |
| 2 | Cahier des charges fonctionnel |
| 3 | Schéma synoptique du procédé |
| 4 | GRAFCET |
| 5 | Liste des entrées/sorties |
| 6 | Architecture logicielle |
| 7 | Documentation des UDT, DB, FB et FC |
| 8 | Programme API |
| 9 | Documentation de simulation PLCSIM |
| 10 | Documentation de supervision WinCC |
| 11 | Cahier des essais et résultats |
| 12 | Guide d'utilisation de la station |

# 15. VALIDATION DU CAHIER DES CHARGES

Le cahier des charges sera considéré comme suffisamment défini pour passer à la conception détaillée lorsque :

- Toutes les fonctions principales seront identifiées.
- Les exigences seront numérotées.
- Les critères de validation seront définis.
- Les contraintes techniques seront connues.
- Les limites du projet seront explicites.
- Les exigences de sécurité auront été prises en compte.

Toute modification importante du procédé devra être répercutée dans le présent document.

---

# 16. CONCLUSION

Ce cahier des charges fonctionnel constitue la référence pour la conception et la réalisation de la station industrielle de dosage de liquide.

Il permettra de guider :

- L'élaboration du GRAFCET.
- La définition des entrées et sorties.
- La conception des structures de données.
- Le développement des FB et FC.
- L'organisation du programme principal OB1.
- La simulation avec S7-PLCSIM.
- La réalisation de la supervision WinCC.
- La validation du fonctionnement global.

**Fin du cahier des charges fonctionnel — Version 1.0.**
