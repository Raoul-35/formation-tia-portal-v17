# LEÇON 01 — ARCHITECTURE D'UN SYSTÈME AUTOMATISÉ

## 1. Définition

Un système automatisé est un ensemble d'équipements permettant
d'effectuer une tâche avec une intervention humaine limitée.

## 2. Architecture générale

Un système automatisé comprend principalement :

- La partie opérative (PO).
- La partie commande (PC).
- L'interface homme-machine (IHM).

## 3. Partie opérative (PO)

Elle représente la partie physique de l'installation.

Elle comprend :

- Les capteurs.
- Les préactionneurs.
- Les actionneurs.
- Les équipements mécaniques et hydrauliques.

## 4. Partie commande (PC)

Elle assure le traitement des informations et la commande
de la partie opérative.

Elle comprend notamment :

- L'automate programmable industriel (API).
- Les modules d'entrées et de sorties.
- Les équipements de communication.

## 5. Interface homme-machine (IHM)

L'IHM permet à l'opérateur de :

- Visualiser l'état de l'installation.
- Commander certains équipements.
- Consulter les alarmes.
- Suivre les paramètres du procédé.

## 6. Exemple industriel : station de pompage

Équipements :

- Capteur de niveau bas.
- Capteur de niveau haut.
- Automate Siemens S7-1200.
- Contacteur moteur.
- Pompe électrique.
- IHM de supervision.

Principe de fonctionnement :

1. Le niveau de la cuve descend.
2. Le capteur informe l'automate.
3. L'automate analyse l'information.
4. L'automate commande le contacteur.
5. Le moteur entraîne la pompe.
6. La cuve se remplit.
7. Le niveau haut est atteint.
8. L'automate arrête la pompe.

## 7. Environnement de formation

- TIA Portal V17.
- STEP 7.
- S7-PLCSIM.
- WinCC Advanced.
- WinCC Runtime Advanced.
- Git et GitHub.

## 8. Conclusion

La compréhension de l'architecture d'un système automatisé
constitue la base de la programmation des automates industriels.
