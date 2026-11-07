# Problèmes à résoudre

Classement pour un hackathon santé B2B. Le détail, les sources et les limites sont dans
`research/sport/04-problemes.md` et `research/sport/06-sources.md`.

## Classement

| # | Problème | Pourquoi c'est solvable par l'IA | Payeur | Preuve |
|---|---|---|---|---|
| 1 | Adhérence entre deux points de soin | transformer une consigne en programme, adapter chaque semaine, relancer au bon moment | employeur / mutuelle | 50–60 % d'abandon, 40 % pour manque de temps |
| 2 | Exercices à domicile après kiné (lombalgie, TMS) | programme court, progression et régression, résumé pour le kiné | employeur | 1 AT sur 5, arrêt moyen ~2 mois, TMS = 30,5 % des arrêts longs |
| 3 | Retour au sport après ACL / blessure | calculer des critères objectifs et refuser une reprise prématurée | mutuelle / employeur | ×7 de rerupture avant 9 mois, −84 % avec les critères |
| 4 | Abandon des programmes d'exercice | réduire la charge, remplacer une séance, changer le format | employeur | 40–70 % d'abandon en salle, ~50 % en home-exercise |
| 5 | Blessures de surcharge du runner débutant | détecter la surcharge et freiner | consommateur | 7,7 à 17,8 blessures/1 000 h, 50–75 % de surcharge |
| 6 | Santé de la femme sportive (RED-S, cycle) | dépistage validé (LEAF-Q, IOC CAT2) et orientation | mutuelle | prévalence 23–79,5 % chez les femmes athlètes |
| 7 | Diabète T2 / risque métabolique | programme lifestyle, suivi | employeur / assureur | −58 % d'incidence, ROI documenté |
| 8 | Burnout via arrêt du sport | détection précoce | employeur | **déjà gagné en avril 2026 : ne pas refaire** |

## Les trois problèmes à retenir

### 1. L'entre-deux des soins

Un patient sort d'une consultation ou d'une série de séances de kiné avec une consigne.
Il reste seul 8 à 12 semaines avant le bilan. C'est là que se jouent l'abandon et la
blessure, et personne n'y est.

Chiffres : ~60 % d'abandon poolé (à revérifier), 40 % attribués au manque de temps,
~50 % en home-exercise, 131 424 personnes en programme APA avec des bilans à 3 et
6 mois mais aucun suivi intermédiaire.

Pourquoi l'IA : le problème n'est pas de générer un programme, c'est de l'**ajuster**
à partir de signaux faibles (une séance manquée, une douleur, une nuit courte) et de
savoir **freiner**. C'est un problème de boucle courte, pas de contenu.

### 2. La lombalgie après les séances

Première cause d'arrêt de travail, arrêt moyen ~2 mois, exercice thérapeutique recommandé
par la HAS mais perdu dès que le patient rentre chez lui. Le kiné ne peut pas vérifier.

Chiffres : 1 AT sur 5, ~409 €/an de kiné remboursés par patient, ~10 séances en moyenne,
TMS = 30,54 % des arrêts longs.

Pourquoi l'IA : un programme de 10 minutes/jour, une question de douleur, une règle de
régression, et un résumé lisible par le kiné à la séance suivante.

### 3. Le retour au sport

La reprise est décidée au ressenti. Les critères validés existent et ne sont pas
appliqués. Un « non » argumenté vaut mieux qu'un plan de plus.

Chiffres : reprise avant 9 mois = ×7 de rerupture ; −51 % de risque par mois de retard
jusqu'à 9 mois ; −84 % avec les critères (LSI ≥ 90 %, ACL-RSI ≥ 80 %) ; seulement
55–65 % retrouvent leur niveau.

Pourquoi l'IA : calculer un score, montrer ce qui manque, générer les semaines de
préparation, et refuser la validation tant que les seuils ne sont pas atteints.

## Problèmes à éviter

- **Détection de burnout par baisse d'activité** : projet gagnant d'avril 2026.
- **Coach running / plan de course** : marché saturé (Strava/Runna, Garmin).
- **Agrégateur de données santé** : saturé en avril.
- **Coach nutrition** : saturé.
- **Chatbot de symptômes** : saturé et risqué réglementairement.
- **Optimisation de la performance** : contraire à la logique d'un assureur.

## Ce qu'il faut prouver dans le pitch

1. Le problème existe et coûte cher (chiffres sourcés).
2. Le produit s'insère dans un parcours existant (clinique, prescription, MSS).
3. L'IA fait quelque chose qu'un formulaire ne fait pas (adaptation, frein, décision).
4. Le payeur économise (arrêts évités, rechutes évitées).
5. La démo marche en 5 minutes, sans API externe.

## Angles libres

- Adhérence post-prescription / post-kiné.
- Retour au sport avec critères objectifs.
- Santé de la femme sportive (RED-S, cycle, post-partum).
- Sécurité de l'activité (savoir freiner) plutôt que performance.
