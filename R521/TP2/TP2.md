---
title: TP2 - Etude expérimentales des filtres
author: Valentin Tugot
date: "18 Décembre 2023"
---

## 1 - Etude du passage d’un signal sinusoïdal à travers un filtre

### 1. Réalisation du filtre

![Filtre passe bas](image.png)

Le filtre à une fréquence de coupure de 20kHz et une plage de transition à 3kHz.

\newpage

### 2. J'ajoute un signal sinusoïdale de fréquence 1kHz

![Signal observé](image-1.png){width=400px}

En sortie du filtre, on obtient un signal de forme sinusoïdale avec une amplitude de 1.

![Schema bloc GNU Radio](image-4.png){width=400px}

\newpage

### 3. Pour différentes fréquences du signal source:

![Signal observé à $f=10kHz$](image-2.png){width=400px}

On a une amplitude de 1.

![Signal observé à $f=20kHz$](image-3.png){width=400px}

Le signal est à la fréquence de coupure du filtre, l'amplitude est desormais de 0.5.

\newpage

![Signal observé à $f=25kHz$](image-5.png){width=400px}

Aucun signel n'est observé en sortie. En effet, 25kHz > 20kHz, le filtre ne laisse pas passer le signal.

## 4. Amplitude du signal en sortie à 20kHz

En sortie du filtre à 20kHz, l'amplitude du signal est de 0.5 (voir Figure 5). En effet, ce résultat est en adéquation avec nos attentes car lorsque l'on se trouve à la fréquence de coupure on perd un gain de 3dB, la puissance est divisé par 2 et l'amplitude divisé par $\sqrt{2}$.

## 5. Bloc Range

J'ai ajouté un bloc range pour les fréquences du signal en entrée et pour la plage de transition du filtre:

![Sliders configurés](image-6.png)

\newpage

## 6. Etude de la plage de transition

![Signal à 16kHz](image-7.png){width=400px}

Pour un signal à 16kHz et une plage de transition à 10kHz, je remarque que l'amplitude du signal diminue déja. Alors que, lorsqu'elle était configuré à 3kHz, il fallait être proche des 20kHz (19kHz par exemple) pour voir l'amplitude du signal diminué.

![Signal à 16kHz, Plage de transition à 1kHz](image-8.png){width=400px}

On remarque que si la plage de transition vaut 1kHz pour ce même signal, l'amplitude reste à 1 et n'a pas encore commencé à diminué.

\newpage

![Signal à 19.5kHz, Plage de transition à 1kHz](image-9.png)

En revanche, pour un signal à 19.5kHz, on remarque que l'amplitude commence à diminuer (elle n'est plus égale à 1). J'en déduis que plus la plage de transition est grande plus le filtre va couper rapidement. Mais, si la plage de transition est faible, le filtre coupera très proche de sa fréquence de coupure on aura une coupure nette.

## 7. Etude d'un filtre passe bande

![Schema Bloc GNU Radio (Passe Bande)](image-10.png){width=400px}

\newpage
J'étudie le comportement de mon signal à travers un filtre passe bande laissant passer les signaux entre 5kHz et 10kHz.

![Signal à 1kHz, Plage de transition à 3kHz](image-11.png){width=400px}

On remarque qu'aucun signal ne passe, c'est normal 1kHz < 5kHz (fréquence de coupure basse du filtre).

![Signal à 1kHz, Plage de transition à 10kHz](image-12.png){width=400px}

En revanche, si je change la fréquence de coupure à 10kHZ, je remarque que j'obtiens un signal en sortie de mon filtre. Ce signal à une amplitude de 0.18.

\newpage

![Signal à 10kHz, Plage de transition à 10kHz](image-13.png){width=400px}

On obtient un signal avec 0.7 d'amplitude, le signal étant à la fréquence de coupure, l'amplitude diminue assez lentement à cause de la plage de transition élevé.

![Signal à 10kHz, Plage de transition à 3kHz](image-14.png){width=400px}

On obtient un signal avec 0.5 d'amplitude. Ici la diminution de l'amplitude est bien plus rapide, la coupure du filtre étant plus nette lorsque la plage de transition est faible.

\newpage

## 8. Combinaison de filtre

![Schema GNU Radio (Combinaison de filtre)](image-15.png){width=400px}

J'ai réalisé le même filtre passe-bande utilisé précédemment mais cette fois-ci en additionnant 2 filtres en série (Laisse passer entre 5kHz et 10kHz). Pour réaliser ce filtre, j'ai d'abord un filtre passe haut avec une fréquence de coupure de 5kHz pour laisser passer tout les signaux supérieurs à 5kHZ puis un filtre passe bas avec une fréquence de coupure de 10kHz qui empêche les signaux supérieur à 10kHz de passer.

![Signal à 7kHz, Plage de transition à 3kHz](image-16.png){width=400px}

On reçoit un signal à 1 d'amplitude, c'est normal puisque 5kHZ < 7kHz < 10kHz, la fréquence du signal est bien au milieu des 2 fréquences de coupure donc le signal passe entièrement.

\newpage

