# Le marché de l'IA appliquée au sport

Objectif : éviter de construire ce que Strava, Garmin et quinze startups font déjà, et
identifier la faille réelle.

## 1. Le mouvement de consolidation

- **Strava a racheté Runna en 2025.** Runna est l'application de plans de course la plus
  visible du marché grand public.
- Runna : environ 19,99 $/mois, plans construits par des coachs (pas générés par IA),
  ajustement des allures selon les performances, mais **pas** selon le sommeil, la HRV,
  la douleur ou la charge réelle.
- Offre groupée Strava + Runna : 149,99 $/an.
- Strava reste la couche sociale et la source de données ; Runna devient la couche plan.

**Conséquence** : un « plan de course personnalisé par IA » en frontal est un produit
commodité, adossé à une marque gratuite (Garmin Coach) ou à un géant (Strava). Ce n'est
pas un angle de hackathon.

## 2. Les concurrents

| Acteur | Proposition | Modèle | Faille |
|---|---|---|---|
| Garmin Coach | plans intégrés à la montre | gratuit avec le matériel | rigide, pas de contexte médical |
| Runna (Strava) | plans de course | abonnement | adapte à la performance passée, pas à la récupération |
| TrainAsONE | plans adaptatifs | abonnement | orienté performance |
| Athletica | entraînement endurance piloté par la physiologie | abonnement | public expert |
| TrainingPeaks | plateforme coach/athlète | abonnement | outil, pas accompagnement |
| Ramp, Empace, Ranner, Rayvik, IntervalCoach, The Running Genie | générateurs de plans | freemium | même faiblesse : la charge, pas la santé |
| Coach Leo | coach IA généraliste | abonnement | pas de données cliniques |

## 3. La faille documentée

La critique commune, reprise par la presse spécialisée en 2026 et par une revue en cours
à Loughborough : **ces applications adaptent le plan à ce que tu as fait, pas à comment
tu récupères**. Elles savent que tu as couru 12 km à 5'30 ; elles ne savent pas que tu
as dormi 5 h, que ton genou a gonflé ou que tu es en déficit énergétique.

C'est précisément l'espace d'un assureur : la donnée de santé, le contexte clinique et
la responsabilité de ne pas blesser.

## 4. Contraintes d'accès aux données

- **Strava API (juin 2026)** : le tier standard est limité à **10 athlètes**, exige un
  abonnement Strava, et applique des rate limits (15 min/jour). L'accès étendu est sur
  review, non garanti.
- **Apple Health** : export XML manuel, pas d'API cloud publique. Utilisable en démo
  avec un fichier fourni, pas en produit.
- **Garmin** : API partenaire, accès conditionné.
- **Wearables grand public** : Fitbit, Withings, Oura ont des API, mais l'accès prend
  du temps et suppose un partenariat.

**Conséquence pour le hackathon** : ne pas bâtir la démo sur une API wearable réelle.
Prévoir un import de fichier (export Apple Health, CSV Strava) et un jeu de données
synthétique pour le tableau de bord.

## 5. Ce qui reste libre

1. **Le sport comme traitement, pas comme performance.** Aucun acteur grand public ne
   gère la prescription médicale, les contre-indications et les critères de retour au
   sport.
2. **La sécurité.** Personne ne dit « arrête cette semaine ». Les apps poussent, elles
   ne freinent pas.
3. **La preuve pour un payeur.** Aucune app grand public ne produit un rapport
   d'adhérence et de sécurité destiné à un employeur ou à une mutuelle.
4. **La santé de la femme sportive.** Cycle, RED-S, post-partum : très peu couvert, et
   les critères de dépistage existent (LEAF-Q, IOC REDs CAT2).
5. **L'entre-deux des soins.** Entre une ordonnance et un bilan, entre deux séances de
   kiné : personne n'occupe le terrain.

## 6. Le piège à éviter

Un jury Alan/Qovery/AWS verra probablement plusieurs projets de « coach IA ». Le
différenciateur ne doit pas être la qualité du plan généré, mais :
- la **contrainte clinique** (pathologie, contre-indication, douleur) ;
- le **garde-fou** (quand ne pas pousser) ;
- la **mesure** (adhérence, critères objectifs, coût évité) ;
- la **conformité** (HDS, minimisation, consentement).

## 7. Infra et conformité

- AWS détient la certification **HDS v2** sur la région **eu-west-3 (Paris)**. Tous les
  services utilisés dans le prototype ne sont pas forcément dans le périmètre HDS, mais
  l'argument « données de santé hébergées sur une infra HDS » est fort pour un assureur.
- **Qovery** déploie sur le compte AWS du client : c'est un control plane, pas un
  hébergeur. Pour le pitch, cela veut dire « déployable dans votre cloud, pas chez nous ».
- Un prototype de hackathon ne traite pas de vraies données de santé. Le préciser évite
  les questions de conformité, et montre qu'on y a pensé.


# Conclusion : 

Je pense qu'il faut éviter les trucs génériques avec simplement des appels API à un LLM