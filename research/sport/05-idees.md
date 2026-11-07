# Idées, scores et recommandation

Cinq pistes issues des problèmes classés dans `04-problemes.md`. Chaque idée est notée
sur 5 critères : impact santé, payeur identifiable, solidité de la preuve, faisabilité
d'une démo en 12 h, différenciation par rapport aux 24 projets d'avril.

| Idée | Impact | Payeur | Preuve | Démo | Diff. | Total /25 |
|---|---|---|---|---|---|---|
| A. L'entre-deux : adhérence après prescription / kiné | 5 | 5 | 4 | 5 | 5 | **24** |
| B. Feu vert : retour au sport avec critères objectifs | 4 | 4 | 5 | 4 | 4 | **21** |
| D. Cycle & charge : santé de la femme sportive | 3 | 3 | 3 | 4 | 5 | **18** |
| C. Alan Play 2.0 : de la marche à la charge | 4 | 4 | 3 | 2 | 4 | **17** |
| E. Frein : détection de surcharge du runner | 3 | 3 | 4 | 5 | 2 | **17** |
| F. Détecteur de burnout via arrêt du sport | 4 | 4 | 3 | 5 | 1 | **17** |

---

## A. L'entre-deux — recommandé

**Nom de travail** : « La séance d'après ».
**Promesse** : « Ce qui se passe entre deux rendez-vous décide du résultat. »

**Problème.** Un patient sort d'une consultation ou d'une série de séances de kiné avec
une consigne d'activité. Il est seul pendant 8 à 12 semaines. 50 à 60 % abandonnent,
et une partie se blesse. Le professionnel ne le sait pas avant le bilan suivant, quand
il est trop tard.

**Utilisateur.** Salarié en arrêt ou en reprise, avec lombalgie ou TMS, orienté par le
kiné ou la clinique Alan.

**Acheteur.** L'employeur (via Alan), parce que les TMS représentent 30,54 % des arrêts
longs et que l'arrêt moyen dépasse 2 mois.

**Parcours de démo (5 minutes).**
1. Le kiné saisit : pathologie, objectif, douleur initiale, date du bilan à 3 mois.
2. L'agent génère 12 semaines, 3 séances/semaine, 10–20 min, sans matériel.
3. Le patient simule 3 semaines : une séance manquée, une douleur à 7/10.
4. L'agent régresse la charge, remplace l'exercice, explique pourquoi.
5. Le kiné ouvre le résumé avant la séance suivante.
6. L'employeur voit : semaines actives, abandons évités, critère de reprise validé.

**Pourquoi ça gagne.**
- Le problème est réel, chiffré, et **personne ne l'occupe** : ce n'est ni un coach, ni
  un chatbot, ni un agenda.
- Le payeur est clair et Alan est déjà dans l'entreprise.
- La démo est robuste : pas besoin d'API wearable, un import de fichier suffit.
- La sécurité est au centre : l'agent sait **freiner**, ce qui est rare et différenciant.

**Risques.**
- **Statut de dispositif médical** : rester sur l'adhérence et l'éducation, ne pas
  diagnostiquer, ne pas prescrire. Formulation : « aide à l'application d'une
  prescription », pas « traitement ».
- **Hallucination clinique** : les règles de progression doivent être déterministes,
  le LLM ne fait que formuler et adapter dans un cadre borné.
- **Scope** : une seule pathologie en démo (lombalgie), pas trois.

**Extension en 30 secondes de pitch.** Le même moteur s'applique au retour au sport
post-ACL (idée B) et aux programmes Maisons Sport-Santé : changer le protocole, pas le
produit.

---

## B. Feu vert — retour au sport avec critères objectifs

**Problème.** Après une ligamentoplastie, la reprise est décidée « au ressenti ». Les
critères validés existent (LSI ≥ 90 %, ACL-RSI ≥ 80 %) mais ne sont pas appliqués.

**Promesse.** « On ne reprend pas parce qu'on se sent prêt. On reprend parce que les
critères sont atteints. »

**Démo.** Un patient à 7 mois veut rejouer au foot. L'agent calcule un score de
readiness, montre les deux critères manquants, génère 5 semaines de préparation, puis
valide à 9 mois.

**Pourquoi c'est fort.**
- Preuve la plus solide de toutes : ×7 de rerupture avant 9 mois, −51 % par mois
  jusqu'à 9 mois, −84 % avec les critères.
- Un « non » de l'IA est plus impressionnant qu'un « oui ».
- Très visuel : jauges, seuils, décision.

**Pourquoi ce n'est pas le choix principal.**
- Population plus étroite (quelques dizaines de milliers d'ACL/an en France).
- Nécessite des tests physiques que le hackathon ne peut pas mesurer réellement ;
  la démo serait déclarative.
- Le payeur est moins évident : le patient, le club, ou la mutuelle.

**Verdict.** Excellent **second vertical** dans le pitch, ou projet principal si
l'équipe a un kiné du sport dans ses rangs.

---

## C. Alan Play 2.0 — de la marche à la charge d'entraînement

**Problème.** Alan Play récompense le volume de pas. Il ne distingue pas une marche
tranquille d'une surcharge, et peut pousser des sédentaires à en faire trop.

**Promesse.** « La même gamification, mais avec un frein. »

**Démo.** Un membre a une semaine à 140 % de sa charge habituelle ; l'agent réduit
l'objectif, remplace le duel par une récupération active, et explique.

**Pourquoi c'est tentant.** Distribution immédiate (200 000 utilisateurs réguliers),
actif existant, histoire simple pour le jury.

**Pourquoi ce n'est pas recommandé.**
- Démo faible : sans intégration réelle à Alan Play, on montre un écran de plus.
- Risque produit : modifier un programme de gamification en 12 h sans données réelles
  est irresponsable, et le jury peut le pointer.
- Le lien avec le payeur (employeur) est moins direct.

**Verdict.** À garder comme **vision** dans le pitch, pas comme produit du jour.

---

## D. Cycle & charge — santé de la femme sportive

**Problème.** Le RED-S et les troubles du cycle chez la sportive sont sous-dépistés, avec
des conséquences osseuses et hormonales durables.

**Promesse.** « Un dépistage validé, pas un diagnostic. »

**Démo.** Questionnaire LEAF-Q + cycle + charge d'entraînement → score de risque →
orientation vers un professionnel → adaptation du programme.

**Pourquoi c'est intéressant.**
- Très peu couvert en avril, forte différenciation.
- Outils validés et publics (LEAF-Q, IOC REDs CAT2) : la démo n'est pas inventée.
- Résonance avec la population Alan et les programmes femmes.

**Limites.**
- Marché plus étroit, payeur moins évident.
- Données intimes : consentement et stockage à traiter explicitement.
- Risque de sur-médicalisation ; il faut un parcours d'orientation clair.

**Verdict.** Le meilleur choix si l'équipe veut se différencier et accepte un marché
plus petit. Peut se greffer sur A comme module.

---

## E. Frein — détection de surcharge du runner

**Problème.** Le runner débutant se blesse par surcharge dans les 3 premiers mois.

**Promesse.** « Le seul coach qui te dit de ne pas courir cette semaine. »

**Démo.** Import d'activités → ratio charge aiguë/chronique → sommeil → verdict
« feu vert / feu orange / feu rouge » → semaine adaptée.

**Pourquoi ce n'est pas recommandé.**
- Marché frontal saturé : Strava/Runna, Garmin Coach gratuit, une dizaine de
  générateurs.
- La différenciation « santé » est réelle mais sera lue comme un coach de plus.
- Le payeur est le consommateur, pas l'employeur.

**Verdict.** À éviter comme projet principal. Utile comme **fonction** dans A ou B.

---

## F. Détecteur de burnout via l'arrêt du sport

**Problème.** Une chute brutale d'activité précède un burnout.

**Pourquoi ce n'est pas une idée neuve pour ce hackathon.** C'est le projet gagnant
d'avril 2026 : « Alan Context », un agent qui téléphone après détection via wearable,
avec la phrase « I just stopped running ». Le jury a déjà récompensé exactement ça.

**Verdict.** **Ne pas refaire.** Si l'équipe veut traiter la santé mentale, prendre
l'angle inverse : l'activité comme traitement adjuvant, avec preuve et parcours.

---

## Recommandation

**Construire A, « La séance d'après »**, avec la lombalgie/post-kiné comme première
verticale, et montrer B (retour au sport) comme extension dans les 30 dernières secondes
du pitch.

Raison en une phrase : c'est le seul angle qui combine un problème massif et chiffré,
un payeur déjà client d'Alan, une démo faisable sans données externes, et un espace
laissé vide par les 24 projets d'avril.

### Ce qui ferait perdre

- Trois fonctionnalités à moitié faites au lieu d'un parcours complet.
- Un beau dashboard sans agent qui prend une décision.
- Aucun chiffre de coût évité dans le pitch.
- Un agent qui ne dit jamais « arrête ».
- Une démo qui dépend d'une API wearable ou d'un compte Strava.

### Plan des 12 h

| Créneau | Objectif |
|---|---|
| 9 h–9 h 45 | Cadrer : une pathologie, un parcours, un rapport. Écrire les règles de progression. |
| 9 h 45–13 h | Moteur : génération du programme, règles de sécurité, adaptation hebdomadaire. |
| 13 h–14 h | Déjeuner + démo du moteur en ligne de commande. |
| 14 h–17 h | Interface patient (3 écrans) + interface kiné (1 écran) + rapport. |
| 17 h–19 h | Jeu de données de démo, scénario de 5 minutes, déploiement Qovery. |
| 19 h–20 h | Pitch, slides, répétition. |
| 20 h–22 h | Buffer : bugs, polish, vidéo de secours. |

### Stack minimale

- **Next.js** (App Router) + Tailwind, déployé via Qovery sur AWS.
- **Amazon Bedrock** pour la génération de texte et l'adaptation, avec un prompt borné
  par des règles déterministes.
- **Postgres** (ou DynamoDB) pour les programmes et les check-ins.
- **Import CSV / JSON** pour l'activité ; pas d'API wearable.
- **Recharts** pour le rapport employeur.
- Une **vidéo de secours** de 90 secondes, enregistrée à 21 h.
