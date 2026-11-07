# Problèmes du sport que l'IA peut résoudre

Classement par intérêt pour un hackathon Alan : taille du problème, existence d'un
payeur, solidité de la preuve, faisabilité d'une démo en 12 h, différenciation par
rapport aux 24 projets d'avril.

| # | Problème | Impact | Payeur | Preuve | Démo 12 h | Différenciation | Total /25 |
|---|---|---|---|---|---|---|---|
| P1 | Adhérence entre deux points de soin | 5 | 4 | 4 | 5 | 5 | **23** |
| P2 | Exercices à domicile après kiné (lombalgie, TMS) | 5 | 5 | 4 | 4 | 4 | **22** |
| P3 | Retour au sport après ACL / blessure | 4 | 4 | 5 | 4 | 4 | **21** |
| P4 | Abandon des programmes d'exercice | 4 | 4 | 4 | 5 | 3 | **20** |
| P5 | Santé de la femme sportive (RED-S, cycle, post-partum) | 3 | 3 | 3 | 4 | 5 | **18** |
| P6 | Diabète T2 / risque métabolique | 5 | 5 | 4 | 2 | 2 | **18** |
| P7 | Blessures de surcharge du runner débutant | 3 | 3 | 4 | 5 | 2 | **17** |
| P8 | Arrêt du sport comme signal de burnout | 4 | 4 | 3 | 5 | 1 | **17** |

P8 est le projet gagnant d'avril (Alan Context) : ne pas le refaire.
P6 est le plus gros marché mais le moins démontrable en 12 h et le plus concurrentiel.

---

## P1. L'adhérence entre deux points de soin

**Le problème.** Un professionnel prescrit de l'activité physique (médecin, kiné,
Maison Sport-Santé) ou termine une rééducation. Le patient repart avec une consigne,
puis reste seul 8 à 12 semaines avant le bilan de contrôle. Personne ne sait s'il a
tenu, s'il s'est blessé, s'il a abandonné.

**Les chiffres.**
- Abandon des programmes d'exercice : **~60 %** en données poolées (revue 2026, à
  revérifier avant citation).
- **40 %** des abandons sont attribués au manque de temps — donc à une charge de
  programme mal calibrée, pas à un manque de volonté.
- Programmes d'exercice à domicile en ligne : **~50 %** d'abandon (étude ~760 sujets,
  9 pays).
- Salles de sport : **40 à 70 %** d'abandon en 6 mois, rétention à 12 mois ~11–19 %.
- L'exercice supervisé réduit l'abandon : **13,2 % vs 20,8 %**.
- Maisons Sport-Santé : 84 784 prescriptions d'APA, 131 424 personnes en programme,
  avec un bilan obligatoire à 3 et 6 mois. Personne n'accompagne entre les bilans.

**Pourquoi ça persiste.** Le professionnel n'a pas le temps ; la structure n'a pas
d'outil de relance ; le patient n'a pas de feedback ; le programme est identique pour
tout le monde.

**Qui paie.** L'employeur (arrêts de travail), la mutuelle (sinistralité), la
collectivité (budget MSS). Pas le patient.

**Ce qu'un agent peut faire.**
- Transformer la prescription en 12 semaines concrètes (jours, durée, intensité,
  matériel disponible).
- Une boucle hebdomadaire courte : 2 questions + données d'activité → adaptation.
- Détecter les signaux d'arrêt (3 semaines sans activité, douleur, sommeil) et relancer
  au bon moment, pas tous les jours.
- Produire un rapport de fin de parcours : adhérence, semaines sécurisées, critères
  atteints.

**Démo.** Écran prescripteur → programme → import d'activité → adaptation visible →
rapport. Tout tient en 5 minutes.

**Risque.** Ce n'est pas un dispositif médical si on reste sur l'adhérence et
l'éducation ; ça le devient si on diagnostique ou si on traite. Le pitch doit le dire.

---

## P2. Les exercices à domicile après une séance de kiné

**Le problème.** La lombalgie est l'un des premiers motifs d'arrêt de travail. Les
exercices thérapeutiques sont recommandés (HAS, grade B pour l'exercice dans la
lombalgie chronique), mais ils sont **perdus après les séances** : le patient a vu le
kiné, a fait les exercices sur place, puis rentre chez lui sans suivi.

**Les chiffres.**
- Lombalgie : **1 accident du travail sur 5**.
- Arrêt moyen : **~2 mois** pour un accident du travail, jusqu'à 1 an dans les formes
  chroniques.
- Kinésithérapie : **~409 €/an remboursés par patient** en 2025, **~10 séances** en
  moyenne.
- Les TMS représentent **30,54 %** des arrêts longs (>31 jours).
- Coût d'un arrêt : plusieurs milliers d'euros par salarié et par an.

**Pourquoi ça persiste.** Une séance de kiné dure 30 minutes, dont une partie en
thérapie manuelle. Le renforcement se fait chez soi, sans matériel, sans correction,
sans progression. Le kiné ne peut pas vérifier.

**Ce qu'un agent peut faire.**
- Programme de 6 à 8 semaines, 10 minutes/jour, sans matériel.
- Suivi déclaratif ultra-court (douleur 0–10, gêne, temps passé).
- Progression automatique et **régression** en cas de douleur.
- Rapport pour le kiné au rendez-vous suivant : ce qui a été fait, ce qui a déclenché
  la douleur.
- Option : comptage de répétitions par webcam (MediaPipe) — spectaculaire en démo,
  mais fragile en 12 h ; à garder comme bonus, pas comme colonne vertébrale.

**Démo.** Un kiné prescrit 3 exercices ; le patient fait la semaine 1 ; l'agent voit
une douleur à 7/10 et remplace l'exercice ; le kiné voit le résumé à la séance 2.

**Pourquoi c'est fort pour Alan.** Alan a déjà un programme « mal de dos » gamifié :
il faut se distinguer par **l'entre-séances** et le lien avec le kiné, pas par le
catalogue d'exercices.

---

## P3. Le retour au sport après une blessure (ACL)

**Le problème.** La reprise du sport après une reconstruction du ligament croisé
antérieur est décidée à la fois trop tôt et sans critères objectifs. Résultat : des
reruptures évitables, et des sportifs qui ne retrouvent jamais leur niveau.

**Les chiffres.**
- Reprise avant **9 mois** : **×7** de risque de rerupture (Grindem et al., BJSM 2016).
- Chaque mois de retard jusqu'à 9 mois : **−51 %** de risque.
- Atteindre les critères (LSI ≥ 90 %, ACL-RSI ≥ 80 %) réduit le risque de **84 %**
  (Kyritsis et al., AJSM 2016).
- Seulement **55 à 65 %** des patients retrouvent leur niveau antérieur.
- Les tests de retour au sport ne sont pas standardisés en pratique courante.

**Pourquoi ça persiste.** Le chirurgien revoit le patient à 3, 6, 12 mois. Entre-temps,
le patient fait ce qu'il peut. Les tests isocinétiques et les questionnaires ne sont pas
faits systématiquement. Le patient « se sent prêt » avant d'être prêt.

**Ce qu'un agent peut faire.**
- Suivre 4 à 6 critères simples (amplitude, saut, questionnaire de confiance, douleur).
- Calculer un score de readiness et **refuser** la reprise si les seuils ne sont pas
  atteints.
- Générer un programme de préparation au retour, semaine par semaine.

**Démo.** Un patient à 7 mois veut reprendre le foot ; l'agent montre un score
incomplet, propose 5 semaines de travail, puis valide à 9 mois avec les seuils.

**Pourquoi c'est fort.** C'est le problème où la preuve est la plus solide et où un
« non » de l'IA a plus de valeur qu'un « oui ». C'est aussi très visuel en démo.

**Limite.** Le marché direct est plus étroit (quelques dizaines de milliers d'ACL/an en
France), mais il s'étend à toutes les reprises post-blessure.

---

## P4. L'abandon des programmes d'exercice en général

**Le problème.** Les gens commencent, puis arrêtent. Le manque de temps est la première
excuse, mais c'est souvent un symptôme : programme trop ambitieux, pas de feedback,
pas de plaisir, pas de lien social.

**Les chiffres.** Voir P1 : 60 % de dropout poolé, 40 % pour manque de temps, 50 % en
home-exercise, 40–70 % en salle.

**Ce qu'un agent peut faire.** Réduire la charge pour tenir, remplacer une séance
manquée, changer le format (10 min au lieu de 45), intégrer un binôme ou une ligue.

**Pourquoi ce n'est pas le bon angle seul.** C'est trop générique : sans pathologie,
sans prescripteur et sans payeur, on retombe sur une app de motivation, marché saturé
et sans ROI démontrable pour Alan.

---

## P5. La santé de la femme sportive

**Le problème.** Le déficit énergétique relatif dans le sport (RED-S) touche une
proportion importante d'athlètes, surtout en endurance, et se manifeste par des
fractures de fatigue, des troubles du cycle, une baisse de performance et des
conséquences osseuses durables. Le dépistage est simple mais peu fait.

**Les chiffres.**
- Prévalence du RED-S : **23 à 79,5 %** chez les femmes athlètes, **15 à 70 %** chez
  les hommes selon les définitions.
- **28 %** des athlètes d'endurance allemandes classées RED-S dans une étude.
- **LEAF-Q** : sensibilité 79 %, spécificité 50 % — bon outil de dépistage, pas un
  diagnostic.
- **IOC REDs CAT2** : protocole de screening validé, utilisable comme base de
  questionnaire.

**Pourquoi c'est intéressant.** Angle très peu pris en avril, forte résonance avec la
population Alan (membres, entreprises, femmes en reprise d'activité), et un questionnaire
validé permet une démo crédible sans données wearable.

**Limites.** Marché plus étroit, sensibilité des données (cycle menstruel), et risque de
sur-médicalisation. À traiter comme un **dépistage + orientation**, pas comme un
diagnostic.

---

## P6. Diabète de type 2 et risque métabolique

**Le problème.** L'activité physique réduit fortement l'incidence du diabète de type 2
et améliore le contrôle glycémique. C'est le cas d'usage le plus évident pour un
assureur, et le mieux documenté.

**Les chiffres.**
- Programme de modification du mode de vie : **−58 %** d'incidence du diabète de type 2
  (Diabetes Prevention Program).
- ROI rapporté : **~4 800 $/personne sur 2 ans**, **>8 000 $ sur 3 ans** (University of
  Michigan).
- Coût d'un salarié diabétique : **2,6×** celui d'un salarié non diabétique.
- **Contre-point important** : un RCT de programmes de bien-être en entreprise n'a pas
  montré de ROI financier à court terme. À citer si le jury challenge le business case.

**Pourquoi ce n'est pas le bon angle pour ce hackathon.** Très concurrentiel
(Noom, Omada, Virta, programmes d'employeurs), difficile à démontrer en 12 h, et
nécessite des données biologiques que le hackathon n'aura pas.

---

## P7. Les blessures de surcharge du runner débutant

**Le problème.** Le débutant qui se met à courir se blesse souvent dans les 3 premiers
mois, puis arrête. La blessure est la première cause d'abandon chez le runner débutant.

**Les chiffres.**
- **7,7 blessures/1 000 h** chez les coureurs récréatifs, **17,8/1 000 h** chez les
  débutants.
- **20 à 79 %** de blessés par an selon les études et les définitions.
- **50 à 75 %** des blessures sont des blessures de surcharge.
- Récidive : **20 à 70 %**.
- Femmes : **5,97 vs 4,68/1 000 h** chez les hommes.

**Pourquoi ce n'est pas le bon angle seul.** Le marché du plan de course est saturé
(Strava/Runna, Garmin gratuit, une dizaine de générateurs). La différenciation par la
santé est possible, mais elle sera lue comme un coach de plus.

---

## P8. L'arrêt du sport comme signal de burnout

**Le problème.** Une baisse brutale d'activité peut précéder un épisode de burnout ou
de dépression. C'est un signal précoce exploitable.

**Pourquoi il ne faut pas le refaire.** C'est exactement le projet qui a gagné l'édition
d'avril : « Alan Context », un agent qui appelle après détection d'un risque de burnout
via wearable, avec la phrase de pitch « I just stopped running ». Le jury a déjà validé
cette idée ; la refaire serait lu comme un manque de recherche.

**Ce qui reste ouvert.** Le lien sport → santé mentale **positive** (activité comme
traitement adjuvant), à condition de ne pas refaire un détecteur de burnout.

---

## Problèmes à ne pas confondre

- **Absentéisme** n'est pas un problème à résoudre, c'est la **conséquence** de P1, P2,
  P3 et P6. C'est la métrique de pitch, pas le produit.
- **Sédentarité** n'est pas un problème de motivation, c'est un problème de **parcours
  et de financement**. Un agent de motivation seul ne change pas l'économie.
- **Performance** n'est pas un problème de santé. Un assureur qui optimise la performance
  prend un risque, il ne le réduit pas.
