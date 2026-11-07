# Sport — thème de recherche n°1

Alan x Qovery x AWS — AI Health Hack 2
Samedi 28 novembre 2026, Paris · 12 h de build · équipes de 4 max · ~70 builders
Candidature avant le **21 novembre 2026, 12 h** · ~6 000 € de prix

## Thèse

Alan vend de la prévention à des employeurs. Le sport est déjà un actif de marque
(Mbappé, Alan Play, la clinique) mais reste un actif **d'image**, pas un actif
**clinique** : Alan mesure des pas, pas une charge d'entraînement, et ne sait rien
de ce qui se passe entre deux rendez-vous de soin.

Le maillon faible du sport-santé n'est ni la prescription ni la motivation initiale.
C'est **l'entre-deux** : les 8 à 12 semaines entre la prescription (médecin, kiné,
Maison Sport-Santé) et le bilan de contrôle. C'est là que 50 à 60 % des gens
abandonnent et que 20 à 79 % des runners se blessent par surcharge.

**Angle recommandé** : un agent qui transforme une prescription d'activité physique
(ou une fin de rééducation) en programme de 12 semaines adaptatif, avec des garde-fous
de sécurité et des critères objectifs de retour au sport. Pas un coach de performance.
Un **dispositif d'adhérence**, vendu à l'employeur ou à la mutuelle.

## Pourquoi le sport, pourquoi Alan

| Actif Alan | Ce qu'il fait | Ce qu'il ne fait pas |
|---|---|---|
| Alan Play (printemps 2025) | gamifie les pas : berries, duels 24 h, ligues, classements | ne mesure pas la charge réelle, ne protège pas de la blessure |
| Kylian Mbappé | crédibilise le sport dans la marque | ne crée aucun parcours produit |
| Clinique Alan (80+ pros) | kiné, diététo, psy, ostéo 7j/7 | ne suit pas le patient entre deux séances |
| Mo + Prevision | assistant 24/7, bilan sanguin 80+ biomarqueurs | pas de données d'activité exploitables |
| Modèle B2B | 41 000 entreprises, 1,1 M membres | le ROI se mesure en arrêts de travail, pas en pas |

Le sport est donc un pilier de communication sans infrastructure de mesure.
C'est exactement le type de trou qu'un hackathon peut combler en 12 h.

## Chiffres clés

| Chiffre | Valeur | Source | Confiance |
|---|---|---|---|
| Coût social de l'inactivité | 140 Md€/an | France Stratégie, 2022 | solide |
| Coût direct Assurance maladie | ~500–559 M€/an | Noël Racine, 2022 | moyen |
| Gain d'un inactif devenu actif | 840 €/an (20–39 ans), 23 275 €/an (40–74 ans) | littérature économique | moyen |
| Français réellement actifs et non sédentaires | ~11 % | études nationales | solide |
| Maisons Sport-Santé habilitées | 580 (fév. 2026), 131 424 personnes en programme APA | Ministère des Sports | solide |
| Abandon des programmes d'exercice | ~60 % (données poolées) | revue 2026 | à revérifier |
| Blessures en course | 7,7/1 000 h (récréatif), 17,8/1 000 h (débutant) | van Gent 2007 ; Kluitenberg 2015 | solide |
| Reprise du sport après ACL avant 9 mois | ×7 de risque de rerupture | Grindem, BJSM 2016 | solide |
| Lombalgie | 1 accident du travail sur 5, arrêt moyen ~2 mois | CNAM / Assurance maladie | moyen |
| Absentéisme France | 5,8 % en 2025, >108 Md€/an | baromètres 2025 | moyen |
| Alan | 1,1 M membres, 41 000 entreprises, NPS +68 | alan.com | solide |

Chaque chiffre est repris avec son contexte et ses limites dans `04-problemes.md` et
`06-sources.md`. Les lignes « à revérifier » ne doivent pas être citées en démo sans
vérification.

## Angles : ce qui est pris, ce qui reste

| Angle | Verdict |
|---|---|
| Coach running / plan d'entraînement personnalisé | **non** — Strava a racheté Runna en 2025, marché frontal saturé |
| Agrégateur de données santé / chatbot symptômes | **non** — saturé en avril (Sentinel, Vital, Hippo, HealthDecoded…) |
| Coach nutrition / recettes | **non** — Nutrimo, Healthy Bites, déjà fait |
| Détection du burnout via l'arrêt du sport | **non** — c'est le projet gagnant d'avril (Alan Context) |
| Adhérence entre deux points de soin | **oui** — recommandé |
| Retour au sport post-blessure / post-ACL | **oui** — critères validés, très démontrable |
| Alan Play 2.0 : de la marche à la charge d'entraînement | **peut-être** — distribution forte, démo faible, risque sécurité |
| Santé de la femme sportive (cycle, RED-S, post-partum) | **oui** — différenciant, marché plus étroit |

Détail des idées, scores et risques : `05-idees.md`.

## La démo en 12 h (si on part sur l'angle recommandé)

1. **Entrée** : un kiné ou un médecin prescrit (écran de saisie simulé) — pathologie,
   objectif, date de bilan à 3 mois.
2. **Programme** : l'agent génère 12 semaines progressives, contraintes par la pathologie
   et par le matériel disponible.
3. **Boucle hebdomadaire** : import Apple Health / saisie manuelle → l'agent adapte la
   semaine suivante, détecte la surcharge, pose 2 questions maximum.
4. **Garde-fou** : douleur, gonflement, nuit blanche → arrêt et orientation vers le soin.
5. **Sortie** : tableau de bord employeur/mutuelle — adhérence, semaines sécurisées,
   critères de retour au sport atteints ou non.

Ce qui rend la démo crédible : les **critères de retour au sport** sont publiés et
mesurables (LSI ≥ 90 %, ACL-RSI ≥ 80 %), pas inventés par le LLM.

## Fichiers

| Fichier | Contenu |
|---|---|
| `01-alan-sport.md` | ce qu'Alan possède déjà, ce que ça implique produit |
| `02-marche-sport-sante.md` | marché français, loi, Maisons Sport-Santé, qui paie |
| `03-marche-ia-sport.md` | concurrents, Strava/Runna, failles, contraintes API |
| `04-problemes.md` | problèmes chiffrés, du plus au moins exploitable |
| `05-idees.md` | 6 pistes scorées, recommandation, risques |
| `06-sources.md` | sources, niveau de confiance, ce qui reste à vérifier |
| `07-moteur-decisionnel.md` | bibliographie commentée du moteur : JITAI, bandits, LLM contraint, évaluation |

## Ce qu'on ne sait pas encore

- Pas de partenaire wearable annoncé cette fois (Thryve était partenaire en avril).
  Prévoir un export Apple Health, une montre perso et un jeu de données synthétique.
- Le règlement exact et les side challenges de cette édition ne sont pas publiés.
- La contrainte HDS ne s'applique peut-être pas à un prototype de hackathon, mais
  l'argument « déployé sur AWS HDS via Qovery » est un différenciateur de pitch.
