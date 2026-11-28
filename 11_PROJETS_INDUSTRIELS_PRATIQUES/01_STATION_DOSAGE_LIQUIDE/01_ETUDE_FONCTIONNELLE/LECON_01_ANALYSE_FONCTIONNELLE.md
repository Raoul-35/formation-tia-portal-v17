# PROJET 01 — STATION INDUSTRIELLE DE DOSAGE DE LIQUIDE

# LEÇON 01 — ÉTUDE ET ANALYSE FONCTIONNELLE

## 1. Présentation générale du projet

### 1.1. Contexte industriel

Le projet consiste à concevoir un système automatisé destiné à préparer un mélange liquide à partir de deux produits différents.

L'installation doit assurer automatiquement :
- Le dosage du liquide A.
- Le dosage du liquide B.
- Le mélange des deux produits.
- La vidange de la cuve.
- La surveillance des équipements.
- La gestion des alarmes et des défauts.

Le système sera développé avec Siemens TIA Portal V17 et simulé à l'aide de S7-PLCSIM. La supervision sera réalisée avec WinCC Runtime Advanced.

### 1.2. Objectif principal

Réaliser une station de dosage automatisée, modulaire et supervisée, capable d'exécuter différentes recettes de production tout en assurant la surveillance du procédé et la gestion des situations anormales.

## 2. Description du procédé industriel

### 2.1. Architecture du procédé

L'installation comprend trois cuves principales :

- TK-101 : réservoir de stockage du liquide A.
- TK-102 : réservoir de stockage du liquide B.
- TK-103 : cuve de mélange.

Le liquide A est transféré depuis TK-101 vers TK-103 par la pompe P-101.

Le liquide B est transféré depuis TK-102 vers TK-103 par la pompe P-102.

Les débitmètres FT-101 et FT-102 mesurent les quantités transférées.

Après le dosage des deux produits, l'agitateur AG-101 homogénéise le mélange.

La vanne XV-103 permet ensuite d'évacuer le produit fini.

### 2.2. Équipements principaux

| Repère | Désignation | Fonction |
|---|---|---|
| TK-101 | Réservoir A | Stockage du liquide A |
| TK-102 | Réservoir B | Stockage du liquide B |
| TK-103 | Cuve de mélange | Réception et mélange |
| P-101 | Pompe A | Transfert du liquide A |
| P-102 | Pompe B | Transfert du liquide B |
| FT-101 | Débitmètre A | Mesure du volume A transféré |
| FT-102 | Débitmètre B | Mesure du volume B transféré |
| AG-101 | Agitateur | Homogénéisation du mélange |
| LT-103 | Transmetteur de niveau | Mesure du niveau de la cuve |
| XV-103 | Vanne de vidange | Évacuation du produit fini |

Des informations complémentaires seront également nécessaires pour assurer la protection des équipements :
- Détection de niveau bas dans les réservoirs sources.
- Détection de niveau haut dans la cuve de mélange.
- Retours de fonctionnement et de défaut des moteurs.
- Information de position de la vanne, si celle-ci est motorisée.
- Arrêt d'urgence.

Ces signaux seront intégrés à la conception détaillée des entrées/sorties.

## 3. Fonctionnement général attendu

Le système doit permettre à un opérateur de sélectionner une recette, de vérifier les paramètres, puis de lancer un cycle de production.

Une recette contient au minimum :
- Le volume de liquide A.
- Le volume de liquide B.
- Le temps de mélange.
- Les limites de fonctionnement autorisées.

Exemple de recette :

| Paramètre | Valeur |
|---|---:|
| Volume A | 100 L |
| Volume B | 50 L |
| Temps de mélange | 120 s |
| Volume total théorique | 150 L |

Ces valeurs sont des exemples pédagogiques et non des caractéristiques définitives d'une installation réelle.

## 4. Description détaillée du cycle automatique

### Étape 1 — Vérification des conditions initiales

Avant tout démarrage, l'automate vérifie :
- L'absence d'arrêt d'urgence.
- L'absence de défaut actif bloquant.
- La disponibilité des deux réservoirs sources.
- La disponibilité de la cuve de mélange.
- La disponibilité des pompes.
- La disponibilité de l'agitateur.
- La disponibilité de la vanne de vidange.
- La validité de la recette sélectionnée.

Si toutes les conditions sont satisfaites, le système autorise le démarrage.

### Étape 2 — Autorisation de démarrage

L'opérateur commande le départ du cycle depuis WinCC.

L'automate vérifie une dernière fois les conditions d'autorisation.

Si elles sont satisfaites, le cycle passe à l'étape de dosage du liquide A.

### Étape 3 — Dosage du liquide A

L'automate commande le démarrage de P-101.

Le débitmètre FT-101 mesure le volume transféré.

La pompe fonctionne jusqu'à ce que le volume mesuré atteigne la consigne de la recette.

Une fois la quantité atteinte :
- La commande de P-101 est retirée.
- Le système vérifie l'arrêt de la pompe.
- Le dosage A est déclaré terminé.

Un temps maximal de dosage doit être prévu pour détecter une anomalie.

### Étape 4 — Dosage du liquide B

L'automate commande le démarrage de P-102.

Le débitmètre FT-102 mesure le volume transféré.

La pompe fonctionne jusqu'à ce que la quantité demandée soit atteinte.

Une fois la quantité atteinte :
- La commande de P-102 est retirée.
- Le système vérifie l'arrêt de la pompe.
- Le dosage B est déclaré terminé.

### Étape 5 — Démarrage de l'agitateur

Après confirmation des deux dosages, l'automate commande AG-101.

Le démarrage du mélange est autorisé uniquement si les conditions nécessaires sont réunies.

### Étape 6 — Mélange

L'agitateur fonctionne pendant la durée définie dans la recette.

À la fin de la temporisation :
- L'agitateur est arrêté.
- Son arrêt est vérifié.
- Le système autorise la vidange.

### Étape 7 — Vidange de la cuve

L'automate commande l'ouverture de XV-103.

Le niveau de la cuve est surveillé par LT-103.

Lorsque le niveau atteint la limite basse de vidange :
- La vanne est refermée.
- La fermeture est vérifiée.
- La fin de vidange est déclarée.

### Étape 8 — Fin du cycle

L'automate :
- Confirme la fin de production.
- Incrémente le compteur de cycles terminés.
- Enregistre les informations de production prévues.
- Rend le système disponible pour une nouvelle recette.

## 5. Modes de fonctionnement

### 5.1. Mode automatique

En mode automatique :
- L'opérateur sélectionne une recette.
- Il lance le cycle.
- L'automate exécute les étapes dans l'ordre prévu.
- Les transitions sont gérées automatiquement.
- Les défauts sont surveillés en permanence.

L'opérateur ne commande pas individuellement les équipements pendant le déroulement normal du cycle.

### 5.2. Mode manuel

En mode manuel, l'opérateur peut commander individuellement :
- P-101.
- P-102.
- AG-101.
- XV-103.

Les commandes manuelles restent soumises aux interverrouillages de sécurité.

Par exemple, une pompe ne doit pas fonctionner si le réservoir source correspondant est vide ou si la cuve de destination présente un niveau maximal.

Le mode manuel ne doit jamais permettre de contourner les protections.

### 5.3. Mode arrêt

Le mode arrêt permet de demander l'arrêt du fonctionnement.

L'automate doit interrompre les commandes de procédé selon une stratégie définie :
- Arrêt des pompes.
- Arrêt de l'agitateur.
- Mise en position sûre de la vanne.
- Interruption de la séquence automatique.

L'état de la vanne et les éventuelles opérations de mise en sécurité devront être définis précisément dans la conception détaillée.

## 6. Gestion des sécurités et interverrouillages

### 6.1. Arrêt d'urgence

L'activation de l'arrêt d'urgence doit provoquer l'arrêt des mouvements et des transferts concernés.

Le redémarrage ne doit pas être automatique après libération de l'arrêt d'urgence.

Il faut :
1. Libérer l'arrêt d'urgence.
2. Identifier et supprimer la cause de l'arrêt.
3. Réarmer le système.
4. Vérifier les conditions initiales.
5. Autoriser un nouveau démarrage.

Dans une installation réelle, la fonction d'arrêt d'urgence doit être conçue avec les équipements de sécurité adaptés. Une simple programmation standard dans l'automate ne remplace pas un circuit de sécurité.

### 6.2. Protection contre le fonctionnement à sec

P-101 ne doit pas fonctionner si TK-101 ne contient pas suffisamment de liquide.

P-102 ne doit pas fonctionner si TK-102 ne contient pas suffisamment de liquide.

### 6.3. Protection contre le débordement

Le niveau de TK-103 doit être surveillé en permanence.

Si le niveau maximal autorisé est atteint :
- Les pompes de transfert sont arrêtées.
- Le dosage est interrompu.
- Une alarme de niveau haut est déclenchée.

### 6.4. Protection contre le fonctionnement anormal des pompes

Chaque pompe doit être surveillée à partir de :
- Sa commande de marche.
- Son retour de fonctionnement.
- Son retour de défaut.
- La présence du débit attendu.

Une absence de débit après une durée définie doit provoquer une alarme ou un défaut de dosage.

### 6.5. Protection de l'agitateur

L'agitateur ne doit démarrer que lorsque les conditions de niveau et de disponibilité sont satisfaites.

Un défaut moteur doit interrompre le mélange et empêcher la poursuite normale du cycle.

## 7. Gestion des défauts

| Code | Défaut | Réaction attendue |
|---|---|---|
| ALM-001 | Défaut pompe A | Arrêt du dosage A |
| ALM-002 | Défaut pompe B | Arrêt du dosage B |
| ALM-003 | Absence de débit A | Interruption du dosage A |
| ALM-004 | Absence de débit B | Interruption du dosage B |
| ALM-005 | Niveau haut cuve | Arrêt des transferts |
| ALM-006 | Défaut agitateur | Interruption du mélange |
| ALM-007 | Défaut transmetteur de niveau | Blocage des opérations dépendantes du niveau |
| ALM-008 | Temps de dosage dépassé | Arrêt du dosage concerné |
| ALM-009 | Arrêt d'urgence | Mise en sécurité du procédé |
| ALM-010 | Défaut vanne de vidange | Interruption ou blocage de la vidange |

Chaque défaut devra disposer :
- D'une condition d'apparition.
- D'une réaction automatique.
- D'une indication sur WinCC.
- D'une condition de disparition.
- D'une procédure de réarmement lorsque nécessaire.

## 8. Gestion des interruptions et des reprises

Une interruption peut survenir pendant :
- Le dosage A.
- Le dosage B.
- Le mélange.
- La vidange.

Le système ne doit pas reprendre automatiquement une opération dangereuse après disparition d'un défaut.

La stratégie retenue sera la suivante :

1. Interrompre les commandes concernées.
2. Conserver l'étape atteinte et les informations utiles au diagnostic.
3. Afficher le défaut sur WinCC.
4. Exiger la suppression de la cause.
5. Autoriser un réarmement.
6. Demander une nouvelle décision de l'opérateur pour reprendre ou abandonner le cycle.

La reprise d'un dosage devra tenir compte du volume déjà transféré. Il ne faudra pas recommencer systématiquement le dosage depuis zéro.

La gestion de cette reprise sera définie lors de la programmation de la séquence.

## 9. Gestion des recettes

Le système devra permettre de gérer plusieurs recettes.

Exemple :

| Paramètre | Recette 01 | Recette 02 | Recette 03 |
|---|---:|---:|---:|
| Volume A (L) | 100 | 80 | 120 |
| Volume B (L) | 50 | 70 | 30 |
| Temps mélange (s) | 120 | 150 | 180 |
| Volume total (L) | 150 | 150 | 150 |

Les recettes devront être validées avant leur utilisation.

Les contrôles porteront notamment sur :
- Les volumes positifs.
- Les volumes inférieurs aux capacités autorisées.
- La cohérence du volume total.
- La durée de mélange autorisée.
- La capacité disponible dans la cuve.

Une recette invalide ne doit pas pouvoir être lancée.

## 10. Informations à afficher sur WinCC

La supervision devra présenter :

### 10.1. Vue générale du procédé
- Les trois cuves.
- Les deux pompes.
- Les deux débitmètres.
- L'agitateur.
- La vanne de vidange.
- Les conduites et le sens des transferts.

### 10.2. Informations de production
- Mode de fonctionnement.
- Étape actuelle du cycle.
- Recette sélectionnée.
- Volume demandé pour A et B.
- Volume réellement transféré pour A et B.
- Temps de mélange restant.
- Nombre de cycles terminés.

### 10.3. Commandes opérateur
- Marche automatique.
- Arrêt du cycle.
- Sélection de recette.
- Réarmement des défauts.
- Commandes manuelles autorisées.

### 10.4. Alarmes
- Alarmes actives.
- Historique des alarmes.
- Date et heure d'apparition.
- État de chaque équipement.

## 11. Exigences fonctionnelles

Le système devra respecter les exigences suivantes :

| Référence | Exigence |
|---|---|
| EF-001 | Le système doit permettre le dosage du liquide A. |
| EF-002 | Le système doit permettre le dosage du liquide B. |
| EF-003 | Les quantités doivent être mesurées par les débitmètres. |
| EF-004 | Le système doit empêcher le débordement de la cuve. |
| EF-005 | Le mélange doit respecter la durée de la recette. |
| EF-006 | La vidange doit être surveillée. |
| EF-007 | Le système doit proposer un mode automatique. |
| EF-008 | Le système doit proposer un mode manuel sécurisé. |
| EF-009 | Les défauts doivent être détectés et signalés. |
| EF-010 | Le système doit permettre le réarmement contrôlé. |
| EF-011 | Les recettes doivent être paramétrables. |
| EF-012 | La production doit être supervisée sur WinCC. |
| EF-013 | Les informations utiles au suivi du cycle doivent être conservées. |
| EF-014 | Le programme doit être organisé de manière modulaire. |

## 12. Limites de l'étude

Le projet sera développé et validé dans un environnement de simulation.

Les éléments suivants seront simulés :
- Fonctionnement des moteurs.
- Mesures de débit.
- Mesure de niveau.
- Positions des vannes.
- Défauts des équipements.
- Arrêt d'urgence.

La simulation permettra de vérifier les séquences, les interverrouillages, les alarmes et la communication avec WinCC.

Elle ne permettra pas de certifier le comportement d'une installation physique ni de valider les performances réelles des équipements.

## 13. Conclusion de l'analyse fonctionnelle

Cette étude définit le fonctionnement attendu de la station industrielle de dosage de liquide.

Elle constitue la référence pour les étapes suivantes :
- Élaboration du GRAFCET.
- Définition de la liste des entrées/sorties.
- Conception des UDT et DB.
- Développement des FB et FC.
- Organisation de OB1.
- Simulation avec S7-PLCSIM.
- Réalisation de la supervision WinCC.

Aucune programmation ne devra commencer avant que les fonctions, les conditions de transition et les réactions aux défauts soient suffisamment définies.
