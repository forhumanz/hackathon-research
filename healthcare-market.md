# Marché et concurrence

Synthèse transversale. Le détail sourcé est dans `research/sport/` :
`02-marche-sport-sante.md` pour le marché français, `03-marche-ia-sport.md` pour les
concurrents IA, `06-sources.md` pour les références.

## 1. Alan en chiffres

| Indicateur | Valeur |
|---|---|
| Membres | 1,1 M |
| Entreprises clientes | 41 000 |
| Pays | 6 |
| Salariés | ~1 000 |
| Rentabilité | atteinte en France |
| Volume | ~1 M de remboursements/mois |
| NPS | +68 |
| Positionnement | « l'assurance prévention » |
| Mission | vivre en bonne santé jusqu'à 100 ans |

Modèle **B2B** : Alan est payé par l'employeur ou l'indépendant. Le retour sur
investissement se mesure en arrêts de travail évités, en sinistralité et en fidélisation,
pas en engagement utilisateur.

### Les actifs prévention

- **Alan Play** (printemps 2025) : gamification des pas, berries, duels 24 h, ligues,
  classements, réductions Alan Shop. 40 %+ des membres ont activé, 200 000+ utilisateurs
  réguliers, +20 000 pas/mois/utilisateur (chiffres annoncés par Alan).
- **Clinique Alan** : 80+ professionnels (kiné, diététo, psy, ostéo), 7j/7 ; 50 % des
  membres n'ont plus besoin d'un rendez-vous physique après une consultation.
- **Mo** : assistant IA 24/7.
- **Prevision** : bilan sanguin, 80+ biomarqueurs.
- **Programme mal de dos** : parcours gamifié déjà existant.
- **Alan as a Service** : offre B2B2C pour les mutuelles.
- **Kylian Mbappé** : ambassadeur, le sport est déjà un pilier de marque.

Le sport est donc présent partout dans la marque, et nulle part dans la mesure
clinique. Alan mesure des pas, pas une charge d'entraînement.

## 2. Le marché français du sport-santé

- Coût social de l'inactivité : **140 Md€/an** (France Stratégie, 2022).
- Coût direct pour l'Assurance maladie : ~500–559 M€/an (périmètre restrictif).
- ~11 % des Français ont un comportement actif et non sédentaire ; 95 % des adultes
  cumulent au moins un facteur de risque.
- **580 Maisons Sport-Santé** (fév. 2026), 131 424 personnes en programme APA,
  84 784 prescriptions, 67 % de femmes.
- L'APA **n'est pas remboursée** par la Sécurité sociale. Certaines mutuelles,
  collectivités et employeurs la financent.
- Le parcours normé comporte des bilans à 0, 3 et 6 mois. Personne n'accompagne entre
  les bilans.

Le marché n'est pas « les sportifs ». C'est **les personnes à qui un professionnel a
déjà dit de bouger et qui ne le font pas seules**. Plus petit, identifiable, solvable.

## 3. Le marché de l'IA appliquée au sport

Saturé sur la performance, ouvert sur la santé.

| Segment | État |
|---|---|
| Plans de course | consolidation : Strava a racheté Runna (2025), Garmin Coach gratuit, une dizaine de générateurs |
| Suivi d'activité | Strava, Garmin, Apple, Polar : verrouillé par les données |
| Nutrition | très concurrentiel (Noom, applications d'employeurs, coachs) |
| Rééducation / retour au sport | peu d'acteurs grand public, beaucoup de cabinets |
| Sport sur ordonnance | quasi vide, porté par des structures publiques sans outil |
| Santé de la femme sportive | très peu couvert |

Faille commune des plans de course : ils adaptent à ce que l'utilisateur a **fait**, pas
à comment il **récupère**. Aucun ne gère la contre-indication, la douleur ou le droit de
dire non.

Contrainte technique : l'API Strava (juin 2026) limite le tier standard à **10 athlètes**
et exige un abonnement. Ne pas bâtir une démo dessus.

## 4. Ce qui a déjà été fait en avril 2026

Édition Alan × Mistral, 11 avril 2026, 72 participants, 24 projets, 5 500 € de prix.

- **Gagnant** : « Alan Context » (Emile Jouannet, solo) — détection du burnout via
  wearable, puis appel téléphonique d'un agent. Pitch : « I just stopped running ».
- **Autres projets** : Sentinel (risque environnemental), Vital (assistant vocal Apple
  Health), Hippo (post-consultation), Mia (fertilité), Signa, Ailan, Clover,
  HealthDecoded, Next Level Rehab, Mora (bras robot), Nutrimo, Coach Spark,
  Albernation, Aura (acouphènes), Babycare, Arwen, Alan Med, CST, Healthy Bites.
- **Side challenges** : Alan Play (Living Avatars, Mo Studios, Personalized Wrapped,
  Health App in a Prompt), Alan Precision (Agentic Health Protocols).

### Angles saturés

- Agrégateurs de données santé.
- Chatbots de symptômes.
- Coachs nutrition.
- Résumés de comptes rendus médicaux.
- Détection de burnout par baisse d'activité (gagnant d'avril).

### Angles libres

- L'adhérence entre deux points de soin (ordonnance, kiné, bilan MSS).
- Le retour au sport avec critères objectifs.
- La santé de la femme sportive.
- La sécurité de l'activité, pas la performance.

## 5. Infra et conformité

- **AWS** détient la certification **HDS v2** sur la région **eu-west-3 (Paris)**.
  Tous les services d'un prototype ne sont pas dans le périmètre, mais l'argument
  « données de santé sur infra HDS » est fort face à un assureur.
- **Qovery** est un control plane : il déploie sur le compte AWS du client. Argument :
  « déployable dans votre cloud, pas chez nous ».
- Le prototype de hackathon ne doit pas traiter de vraies données de santé. Le dire
  montre qu'on y a pensé.

## 6. Implications

1. Le payeur est l'employeur, pas le patient.
2. La preuve attendue est une économie d'arrêts de travail, pas un score d'engagement.
3. Le sport est déjà un actif de marque : le projet doit s'y brancher, pas le dupliquer.
4. Le marché de la performance est fermé ; celui de la prescription est ouvert.
5. La conformité et la sécurité sont des arguments de vente, pas des cases à cocher.
