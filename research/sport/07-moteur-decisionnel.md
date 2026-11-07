# Moteur décisionnel — ressources commentées

Objectif : construire un moteur qui décide **quand** interrompre, **quoi** proposer,
**comment** le formuler, et **ce qu'il retient** d'un refus. Cette page classe la
littérature par couche du moteur, avec pour chaque ressource ce qu'elle apporte et
son statut (à lire avant le hackathon / référence / optionnel).

> **Si tu ne lis que cinq choses** : Nahum-Shani 2018 (JITAI), Klasnja 2018
> (HeartSteps, les chiffres qui font mal), Russo 2018 (Thompson sampling),
> Song 2025 (bandit × LLM en conditions réelles), et la doc « structured outputs »
> d'Anthropic. Le reste est du renfort.

---

## 0. Le modèle mental en une page

Notre moteur n'est pas « un LLM qui décide ». C'est une chaîne de cinq étages, et
chaque étage a une littérature distincte :

| Étage | Question | Littérature | Notre choix probable |
|---|---|---|---|
| 1. Disponibilité | Est-ce un moment acceptable ? | JITAI, interruptibilité | filtre déterministe (agenda, déjà actif, DND) |
| 2. Sélection | Quel type d'action ? | bandits contextuels | Thompson sampling sur 6–8 catégories × créneaux |
| 3. Formulation | Quels mots exactement ? | LLM + santé | LLM contraint par un catalogue fermé |
| 4. Validation | Est-ce légal et sûr ? | sécurité, JSON Schema | couche déterministe + validation de schéma |
| 5. Mémoire | Qu'est-ce qu'on apprend ? | event sourcing, stats bayésiennes | `profile.json` + `events.jsonl` + patch validé |

Le point important : **le LLM n'occupe que l'étage 3**. Les étages 1, 2, 4 et 5
sont déterministes et testables. C'est ce qui rend le système démontrable en 12 h
et défendable devant un jury.

---

## 1. Le cadre scientifique : JITAI et micro-essais randomisés

C'est la colonne vertébrale conceptuelle. Un JITAI = une intervention qui choisit
**le bon moment**, **le bon contenu**, **la bonne intensité**, et qui sait **s'arrêter**.

### Nahum-Shani et al. 2018 — *Just-in-Time Adaptive Interventions in Mobile Health*
- Lien : https://pmc.ncbi.nlm.nih.gov/articles/PMC5364076/
- Statut : **à lire avant le hackathon** (le papier fondateur, ~15 pages).
- À retenir : les sept composants d'un JITAI — *decision points*, *tailoring
  variables*, *intervention options*, *decision rules*, *proximal outcomes*,
  *distal outcomes*, *receptivity*. Et la notion de **burden** : une intervention
  mal timée coûte plus qu'elle ne rapporte.
- Usage direct : notre doc de design doit remplir ces sept cases. Si une case est
  vide, le jury la trouvera.

### Klasnja et al. 2018 — *Efficacy of Contextually Tailored Suggestions for Physical Activity* (HeartSteps)
- Lien : https://pmc.ncbi.nlm.nih.gov/articles/PMC6401341/
- Statut : **le papier le plus important pour nous**, parce qu'il chiffre ce qu'on
  est en train de promettre.
- Design : 6 semaines, 44 participants, 5 points de décision/jour, randomisation
  micro à chaque point (suggestion vs rien ; marche vs anti-sédentarité).
- Résultats qui doivent changer notre design :
  - une suggestion de marche augmente les pas des 30 min suivantes de **+24 %**
    (59 pas sur une base de 253) ;
  - l'effet est **+107 % au jour 1**, puis décroît de **3 % par jour** et n'est
    plus détectable vers le **jour 29** → habituation ;
  - les suggestions **anti-sédentarité** sont très appréciées… mais n'ont **aucun
    effet détectable sur les pas**.
- Conséquence pour nous : notre métrique ne peut pas être « pas » si notre action
  est « lève-toi 2 minutes ». Il faut mesurer la **pause assise interrompue**
  (auto-déclarée ou accéléromètre), sinon on reproduit l'angle mort de HeartSteps.
  Et il faut traiter la **nouveauté comme une ressource périssable** : rotation,
  plafond de fréquence, retrait des actions saturées.

### Klasnja et al. 2015 — *Micro-Randomized Trials: An Experimental Design for Developing JITAIs*
- Lien : https://pmc.ncbi.nlm.nih.gov/articles/PMC4732571/
- Statut : référence méthodo.
- À retenir : on randomise **à chaque point de décision**, pas par personne. C'est
  ce qui permet d'attribuer un effet au bon moment. Pour la démo : simuler un MRT
  est plus crédible qu'un A/B test sur 20 utilisateurs.

### Liao et al. 2019 — *Personalized HeartSteps: A Reinforcement Learning Algorithm*
- Lien : https://arxiv.org/abs/1909.03539
- Statut : **à lire** si tu veux l'architecture de référence.
- À retenir : le reward est défini sur l'activité **après** la suggestion, avec un
  terme de **burden** (coût d'envoyer une notification). L'algorithme apprend
  aussi **quand ne pas envoyer**. C'est exactement notre problème.
- Détail utile : la disponibilité est un filtre *avant* la politique (marche en
  cours, conduite, hors ligne → pas de suggestion). On peut copier ce principe
  sans copier le modèle.

---

## 2. Décider sous incertitude : bandits contextuels

Notre problème est un **contextual bandit**, pas du RL complet : on choisit une
action, on observe une récompense immédiate, pas de crédit à long terme complexe.
Le dire explicitement au jury est un signe de maturité.

### Russo et al. 2018 — *A Tutorial on Thompson Sampling*
- Lien : https://arxiv.org/abs/1707.02038
- Statut : **à lire avant le hackathon** (chapitres 1–2 suffisent).
- À retenir : l'algorithme tient en 3 lignes. Pour chaque action, on maintient une
  croyance (Beta-Bernoulli pour un succès/échec), on échantillonne, on prend le
  meilleur échantillon. L'exploration est **automatique** et proportionnelle à
  l'incertitude. Pas de paramètre epsilon à régler.
- Pourquoi c'est le bon choix en 12 h : implémentation ~30 lignes, interprétable,
  fonctionne avec très peu de données, et se raconte en une slide.

### Lattimore & Szepesvári — *Bandit Algorithms* (livre gratuit)
- Lien : https://tor-lattimore.com/downloads/book/book.pdf
- Statut : référence, à ne pas lire linéairement.
- À retenir : chapitres 1–2 pour le vocabulaire (regret, exploration, adversarial
  vs stochastic), chapitre 36 pour le cadre contextual. Le reste est pour plus tard.

### Chapelle & Li 2011 — *An Empirical Evaluation of Thompson Sampling*
- Lien : https://papers.nips.cc/paper/2011/hash/e53a0a2978c28872a4505bdb51db06dc-Abstract.html
- Statut : optionnel, mais bonne munition pour le pitch.
- À retenir : TS est compétitif ou meilleur qu'UCB sur la publicité display et la
  recommandation d'articles, et **plus robuste aux retours retardés ou groupés**.
  C'est exactement notre cas : l'utilisateur répond 40 minutes plus tard, parfois
  le lendemain.

### Li et al. 2010 — *A Contextual-Bandit Approach to Personalized News Recommendation* (LinUCB)
- Lien : https://arxiv.org/abs/1003.0146
- Statut : référence.
- À retenir : si on veut du contexte continu (heure, énergie, sommeil, agenda),
  LinUCB / LinTS est la version linéaire. Mais attention : avec 20 utilisateurs et
  30 jours, un modèle linéaire par action est déjà trop gourmand. Rester sur des
  bras discrets (catégorie × créneau) est plus honnête.

### Sutton & Barto — *Reinforcement Learning: An Introduction* (gratuit)
- Lien : http://incompleteideas.net/book/the-book-2nd.html
- Statut : culture générale, **pas nécessaire pour le hackathon**.
- À retenir : le cadre MDP sert à expliquer pourquoi notre problème est *plus
  simple* qu'il n'y paraît. Ne pas partir sur du PPO ou du DQN : ce serait un
  mauvais signal (sur-ingénierie, pas de données).

---

## 3. LLM dans la boucle : les quatre architectures possibles

C'est ici que se joue le choix A (LLM-policy) vs B (hybride). La littérature est
étonnamment claire : **B gagne**, et pas seulement en coût.

### Haag et al. 2025 (CHI) — *The Last JITAI?*
- Lien : https://arxiv.org/abs/2402.08658
- Statut : **à lire** (c'est le papier qui justifie d'utiliser un LLM pour autre
  chose que du texte).
- Design : GPT-4 génère 450 décisions JITAI (3 personas × 5 contextes), évaluées
  par 10 profanes et 10 professionnels de santé, contre des décisions humaines.
- Résultat : les JITAI générées par GPT-4 sont jugées **meilleures** que celles des
  deux groupes d'humains sur l'adéquation, l'engagement, l'efficacité perçue et le
  professionnalisme.
- Limites à citer honnêtement : cadre **conceptuel** (rééducation cardiaque
  simulée), évaluation par **jugement humain**, aucun outcome clinique. Ce n'est
  pas une preuve d'efficacité, c'est une preuve de **plausibilité du contenu**.

### Karine & Marlin 2025 — *Combining LLM decision and RL action selection*
- Lien : https://arxiv.org/abs/2501.06980
- Statut : **à lire** (c'est notre variante B, formalisée).
- Architecture : le bandit RL propose des actions ; le LLM sert de **filtre** qui
  intègre les préférences exprimées en texte libre ; la décision finale combine
  les deux.
- À retenir : le LLM est bon pour **lire une préférence en langage naturel**
  (« je déteste sortir le matin », « j'ai mal au genou »), pas pour **arbitrer
  l'exploration**. Séparer les deux rôles est ce qui rend le système testable.
- Leur environnement de simulation génère des préférences textuelles et modélise
  les contraintes : c'est un bon patron pour notre jeu de données synthétique.

### Song et al. 2025/2026 — *Tailored Behavior-Change Messaging for Physical Activity: Integrating Contextual Bandits and LLMs*
- Lien : https://arxiv.org/abs/2506.07275
- Statut : **le papier le plus proche de notre produit**. À lire en priorité.
- Design : intervention de **30 jours** sur l'activité physique, cinq bras
  (randomisation égale, bandit seul, LLM seul, LLM + historique, **bandit × LLM**).
  Le bandit choisit le **type** d'intervention (auto-monitoring, gain-framing,
  loss-framing, comparaison sociale) ; le LLM **personnalise le message** à
  l'intérieur de ce type.
- Résultat : l'approche hybride **conserve l'acceptabilité perçue** des messages
  générés par LLM, tout en **réduisant les tokens** et en fournissant une **règle
  de décision explicite et reproductible**. Elle corrige aussi le biais de
  distribution entre types d'intervention.
- C'est exactement l'argument qu'il nous faut : le LLM seul est cher, non
  reproductible et impossible à auditer ; le bandit seul produit des messages
  répétitifs. L'hybride fait mieux sur les trois axes.

### Brooks et al. 2025 (NeurIPS) — *Generator-Mediated Bandits* (GAMBITTS)
- Lien : https://arxiv.org/abs/2505.16311
- Statut : référence théorique, optionnel pour le hackathon.
- À retenir : quand un LLM génère le contenu, l'action réellement délivrée est
  **stochastique**. Un bandit classique ignore cette source d'incertitude et
  apprend moins vite. GAMBITTS modélise séparément le **traitement** (le texte
  généré) et la **récompense**. Donne des bornes de regret.
- Usage : si on garde le LLM comme générateur, on peut au moins **logger le texte
  exact délivré** et l'utiliser comme variable, plutôt que de traiter « action =
  catégorie » comme si le texte était constant.

### Alamdari et al. 2024 (EMNLP) — *Jump Starting Bandits with LLM-Generated Prior Knowledge*
- Lien : https://arxiv.org/abs/2406.19317
- Code : https://github.com/BorealisAI/jump-starting-bandits
- Statut : **très utile pour le cold start**.
- À retenir : on peut demander au LLM de **simuler des utilisateurs** et leurs
  préférences pour pré-entraîner le bandit, ce qui réduit le regret initial. C'est
  la réponse au problème « on n'a aucune donnée au jour 1 ».
- Prudence : les priors du LLM sont des **hypothèses**, pas des données. Les
  logger comme tels, et les laisser être écrasés par les vrais retours.

### Bouneffouf & Feraud 2025 — *Survey: Multi-Armed Bandits Meet Large Language Models*
- Lien : https://arxiv.org/abs/2505.13355
- Statut : carte du domaine, à parcourir 20 minutes.
- À retenir : deux directions distinctes — les bandits pour **optimiser** le LLM
  (prompt, fine-tuning) et les LLM pour **améliorer** les bandits (contexte,
  priors, raisonnement). Nous sommes dans la seconde.

### Veille bibliographique
- https://github.com/bucky1119/Awesome-LLM-Bandit-Interaction
- Statut : à garder en favori. Liste à jour des papiers LLM × bandits, classée par
  thème (reward, exploration, décision, recommandation).

---

## 4. Contraindre le LLM : sorties structurées et validation

Le point le plus opérationnel. Un LLM qui renvoie du texte libre est inutilisable
dans un moteur de décision. On veut un **objet validé**, avec un identifiant
d'action appartenant à un catalogue fermé.

### Anthropic — *Structured outputs* et *strict tool use*
- Lien : https://platform.claude.com/docs/en/build-with-claude/structured-outputs
- Lien : https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview
- Statut : **à lire avant d'écrire le prompt**.
- À retenir : `output_config.format.schema` contraint la réponse à un JSON Schema ;
  `tools[].strict = true` contraint les arguments d'un outil. Les deux utilisent
  un pipeline de grammaire, donc la sortie est **garantie syntaxiquement
  conforme** — mais pas sémantiquement correcte. Il faut quand même valider les
  valeurs (énumérations, bornes, cohérence).
- Limites du schéma : `additionalProperties: false` obligatoire pour les objets,
  pas de contraintes numériques ou de longueur de chaîne. Donc les bornes
  (`1 ≤ intensité ≤ 5`) se vérifient **après** génération.

### OpenAI — *Structured outputs*
- Lien : https://platform.openai.com/docs/guides/structured-outputs
- Statut : équivalent, à comparer si on hésite sur le modèle.
- À retenir : même idée (JSON Schema strict), mais les fonctionnalités supportées
  diffèrent légèrement. Le schéma doit être écrit **au plus petit dénominateur
  commun** si on veut pouvoir changer de fournisseur.

### JSON Schema
- Lien : https://json-schema.org/understanding-json-schema/
- Statut : référence.
- À retenir : écrire le schéma **avant** le prompt. C'est le contrat. Le schéma
  doit contenir : `action_id` (enum fermé), `variant_id` (enum), `message`
  (string, max ~240 caractères), `rationale` (enum court, pas du texte libre),
  `confidence` (0–1), `profile_patch` (objet aux clés autorisées),
  `profile_version` (entier).

### Validation côté code
- **Zod** (TypeScript) : https://zod.dev
- **Pydantic** (Python) : https://docs.pydantic.dev
- **Instructor** : https://python.useinstructor.com — wrappe le retry et la
  validation de schéma.
- **Outlines** : https://github.com/dottxt-ai/outlines — génération contrainte,
  utile si on auto-héberge.
- À retenir : le pipeline est **generate → validate → repair once → fallback
  déterministe**. Jamais plus d'un retry : au-delà, on masque un problème de
  conception derrière de la latence.

### Sécurité du prompt
- Les raisons de refus sont du **texte utilisateur**. Elles ne doivent jamais être
  interprétées comme des instructions. Deux parades : (1) les classer dans une
  enum fermée avec un appel séparé et un prompt qui ne contient que la phrase ;
  (2) ne jamais injecter la phrase brute dans le prompt de génération, seulement
  la classe obtenue.
- Référence courte : OWASP LLM Top 10, *Prompt Injection*
  https://genai.owasp.org/llmrisk/llm01-prompt-injection/

---

## 5. État, mémoire et apprentissage

Le piège classique : laisser le LLM réécrire toute la fiche utilisateur. On perd
la traçabilité, on ne peut plus expliquer un changement, et on ne peut pas
régresser.

### Event sourcing (Martin Fowler)
- Lien : https://martinfowler.com/eaaDev/EventSourcing.html
- Statut : **à lire** (10 minutes).
- À retenir : on stocke les **faits** (`events.jsonl`), pas l'état. L'état
  (`profile.json`) est une **projection** recalculable. Un bug de projection se
  corrige en rejouant les événements ; un bug d'état est irréparable.
- Application : `events.jsonl` contient `proposed`, `accepted`, `refused`,
  `completed`, `abandoned`, `pain_reported`, avec timestamp, créneau, action,
  raison classée, et **propension** (probabilité avec laquelle l'action a été
  choisie — indispensable pour l'évaluation offline).

### JSON Patch (RFC 6902) et JSON Merge Patch (RFC 7396)
- Lien : https://datatracker.ietf.org/doc/html/rfc6902
- Lien : https://datatracker.ietf.org/doc/html/rfc7396
- Statut : référence d'implémentation.
- À retenir : le LLM renvoie un **patch**, pas un document complet. Un réducteur
  applique uniquement les chemins autorisés (`/preferences/actions/*/affinity`,
  `/constraints/*`). Toute clé hors liste blanche est rejetée. C'est la différence
  entre « le LLM propose » et « le LLM décide ».

### Concurrence et idempotence
- `profile_version` en optimistic locking : le patch porte la version lue ; s'il
  y a eu un autre événement entre-temps, on rejette et on rejoue.
- `event_id` idempotent : un retry réseau ne doit pas compter deux fois un refus.
- À retenir : sans ça, le compteur « 3 refus consécutifs » devient faux dès le
  premier doublon, et la règle de suppression de créneau se déclenche à tort.

---

## 6. Évaluer sans données réelles

On n'aura pas de vrais utilisateurs. Il faut donc distinguer trois choses :
**démontrer que ça apprend**, **démontrer que c'est sûr**, et **ne pas prétendre
que c'est efficace**.

### Évaluation offline (off-policy evaluation)
- **Open Bandit Pipeline** : https://zr-obp.readthedocs.io/en/latest/
- Papier : https://arxiv.org/abs/2008.07146
- Statut : **à parcourir**, surtout pour le vocabulaire.
- À retenir : IPS (inverse propensity scoring), SNIPS, doubly robust. On ne peut
  évaluer une nouvelle politique que si on a loggé **la propension** de chaque
  action. C'est la raison pour laquelle le log doit contenir `p(action choisie)`.
- Piège : si on a toujours joué l'action « gourmande », on n'a aucune donnée sur
  les autres → l'évaluation offline est biaisée. D'où l'exploration forcée.

### Simulation
- **River** (bandits en streaming) : https://riverml.xyz/latest/api/bandit/
- **Vowpal Wabbit** : https://vowpalwabbit.org
- **MabWiser** : https://github.com/fidelity/mabwiser
- **Open Bandit Pipeline** pour les estimateurs OPE.
- À retenir : un simulateur encode **nos hypothèses**. Il sert à faire des tests de
  régression (« le moteur apprend-il quand le monde est stationnaire ? »,
  « respecte-t-il le refus ? »), pas à prouver l'efficacité santé. Le dire au jury
  avant qu'il ne le demande.

### Métriques à définir maintenant
| Type | Métrique | Pourquoi |
|---|---|---|
| Proximal | action complétée dans les 10 min | mesure l'effet immédiat, comme HeartSteps |
| Distal | pauses actives / semaine | le KPI produit |
| Distal | semaines avec ≥ 5 pauses | rétention comportementale |
| Guardrail | taux de refus | sur-sollicitation |
| Guardrail | refus consécutifs par créneau | la règle des 3 |
| Guardrail | notifications ignorées | fatigue |
| Apprentissage | regret cumulé vs baseline aléatoire | prouve que le bandit sert à quelque chose |
| Apprentissage | couverture des actions | évite l'effondrement sur un bras |

### Piège statistique
Avec 20 utilisateurs simulés et 30 jours, on ne détecte pas un effet de 5 %. La
démo doit montrer une **courbe d'apprentissage** (regret qui décroît, préférences
qui se stabilisent) et des **cas de refus respectés**, pas des p-values.

---

## 7. Sciences du comportement : pourquoi les gens refusent

Le bandit optimise ce qu'on lui donne comme récompense. Si la récompense est
« clic », on obtient une app de clics. Il faut donc choisir un modèle de
comportement avant de coder la récompense.

### Fogg — *Behavior Model* (B = MAP)
- Lien : https://behaviormodel.org/
- Statut : **à lire** (10 minutes, c'est une page).
- À retenir : un comportement arrive quand **Motivation × Ability × Prompt** se
  rencontrent au même moment. Trois leviers : rendre l'action plus facile
  (ability), la rendre plus désirable (motivation), ou changer le déclencheur
  (prompt). Notre produit agit surtout sur **ability** et **prompt**.
- Application directe : si un utilisateur refuse « trop dur », on baisse
  l'intensité (ability), on ne change pas l'heure. S'il refuse « pas envie », on
  change le type d'action (motivation), pas le créneau. C'est notre règle actuelle,
  et elle est cohérente avec le modèle.

### Michie et al. 2011 — *The Behaviour Change Wheel* (COM-B)
- Lien : https://link.springer.com/article/10.1186/1748-5908-6-42
- Statut : référence solide pour le pitch.
- À retenir : **Capability, Opportunity, Motivation → Behavior**. Neuf fonctions
  d'intervention (education, persuasion, incentivisation, coercion, training,
  restriction, environmental restructuring, modelling, enablement).
- Usage : notre catalogue d'actions doit couvrir plusieurs fonctions, pas
  seulement « persuasion ». Sinon on optimise un seul levier et on sature vite.

### Ryan & Deci 2000 — *Self-Determination Theory*
- Lien : https://doi.org/10.1037/0003-066X.55.1.68
- Statut : optionnel.
- À retenir : autonomie, compétence, affiliation. Le droit de refuser n'est pas
  une perte d'engagement, c'est ce qui préserve l'autonomie — donc la rétention.
  C'est l'argument théorique qui justifie notre bouton « pas maintenant ».

### Lally et al. 2010 — *How are habits formed*
- Lien : https://onlinelibrary.wiley.com/doi/10.1002/ejsp.674
- Statut : **à citer avec précaution**.
- À retenir : médiane de **66 jours** pour atteindre 95 % du plateau d'automaticité,
  avec une variabilité énorme (18–254 jours). Le « 21 jours » est un mythe.
- Usage : ne pas promettre un habit en 30 jours. Promettre une **réduction de la
  friction** et mesurer l'automaticité perçue.

### Wood & Neal 2007 — *A New Look at Habits and the Habit–Goal Interface*
- Lien : https://doi.org/10.1037/0033-295X.114.4.843
- Statut : optionnel.
- À retenir : les habitudes sont déclenchées par le **contexte**, pas par la
  volonté. D'où l'importance du **créneau** (contexte stable) et des
  **implementation intentions** (« si je suis à 14 h devant mon écran, alors je me
  lève »).

### Gollwitzer 1999 — *Implementation Intentions*
- Lien : https://doi.org/10.1037/0003-066X.54.7.493
- Statut : optionnel mais très applicable.
- À retenir : une intention « si X alors Y » augmente significativement le passage
  à l'action. On peut la faire formuler à l'onboarding en une question.

### Eysenbach 2005 — *The Law of Attrition*
- Lien : https://www.jmir.org/2005/1/e11/
- Statut : **à lire** (court).
- À retenir : dans les eHealth, l'attrition est massive et souvent mal mesurée.
  Distinguer **attrition d'usage** et **attrition de santé** (les gens vont mieux
  et partent). C'est une objection classique en jury, et une réponse honnête.

### Mehrotra et al. 2016 — *My Phone and Me: Understanding People's Receptivity to Mobile Notifications*
- Lien : https://doi.org/10.1145/2858036.2858566
- Statut : **à parcourir**.
- À retenir : la réceptivité dépend du **contexte**, du **contenu**, de
  l'**expéditeur** et de la **personnalité**. Les notifications sont jugées plus
  positivement quand elles arrivent **entre deux activités** (transition) que
  pendant une tâche focalisée.
- Application : notre détecteur de « trou d'agenda » est une proxy de transition.
  C'est exactement le bon signal, et on peut le dire avec une référence.

### Pielot et al. 2014 / 2017 — notifications et moments opportuns
- Lien : https://pielot.org/pubs/Pielot2017-UbiComp-Engagement.pdf
- Statut : optionnel.
- À retenir : 60–100 notifications/jour ; les gens répondent en quelques minutes
  mais les trouvent intrusives. Le **coût d'attention** est réel. Un produit qui
  ajoute des notifications doit prouver qu'il retire du bruit, pas qu'il en ajoute.

---

## 8. Sécurité : ce que le moteur n'a pas le droit de faire

C'est la couche qui doit être **déterministe** et **hors du LLM**. Le LLM ne doit
jamais pouvoir proposer une action absente du catalogue, ni ignorer un drapeau
rouge.

### PAR-Q+ / ePARmed-X+
- Lien : https://eparmedx.com/
- Statut : **à lire avant de définir l'onboarding**.
- À retenir : 7 questions de dépistage, puis des questions de suivi par pathologie,
  avec trois niveaux de risque (faible, intermédiaire, élevé). C'est le standard
  international, gratuit, traduit en français.
- Usage : notre questionnaire d'entrée doit être une **version courte et assumée**
  du PAR-Q+, pas un formulaire inventé. Et il doit produire un **niveau de risque**
  qui filtre le catalogue.

### OMS 2020 — *Guidelines on Physical Activity and Sedentary Behaviour*
- Lien : https://www.who.int/publications/i/item/9789240015128
- Statut : **référence pour les seuils**.
- À retenir : 150–300 min d'activité modérée par semaine, 2 séances de renforcement,
  et **réduire le temps assis** indépendamment du volume d'activité. C'est la
  justification scientifique des micro-pauses : c'est un comportement distinct,
  pas un sous-produit des pas.

### ACSM — *Exercise Preparticipation Health Screening*
- Lien : https://www.acsm.org/education-resources/trending-topics-resources/exercise-pre-participation-health-screening
- Statut : référence.
- À retenir : la logique de stratification (symptômes, maladie connue, niveau
  d'activité) et les **signaux d'arrêt** : douleur thoracique, syncope, dyspnée
  anormale, palpitations, douleur articulaire aiguë.
- Application : la douleur déclarée doit **arrêter** l'action et orienter, pas
  être « adaptée » par le LLM. Règle simple : douleur ≥ 6/10 ou nouvelle douleur
  → stop + message de sécurité + proposition de contact soignant.

### Statut réglementaire (MDR, règle 11)
- Point de vigilance : un logiciel qui **diagnostique**, **prescrit** ou **traite**
  peut devenir dispositif médical. Rester sur « aide à l'application d'une
  recommandation » et « éducation », ne pas calculer de dose d'exercice
  thérapeutique, ne pas prétendre soigner.
- À mentionner en une phrase dans le pitch : c'est un signe de sérieux, pas un
  frein.

---

## 9. Outils : quoi utiliser pour quoi

| Besoin | Outil | Remarque |
|---|---|---|
| Bandit Beta-Bernoulli | ~30 lignes maison | préférable à une lib pour la démo : auditable |
| Bandit contextuel linéaire | River, MabWiser | si contexte continu |
| Évaluation offline | Open Bandit Pipeline | IPS / DR, nécessite les propensions |
| Génération contrainte | Instructor, Outlines | si le fournisseur ne fait pas de schéma strict |
| Validation de schéma | Pydantic / Zod | le contrat, pas un accessoire |
| Event log | JSONL append-only | lisible, diffable, rejouable |
| Patch | RFC 6902 (jsonpatch) | liste blanche de chemins |
| Simulation | Python + LLM personas | tests de régression, pas preuve |
| Dashboard démo | Streamlit / Next.js | montrer la courbe d'apprentissage |

---

## 10. Trois parcours de lecture selon le temps disponible

### 2 heures (avant le hackathon)
1. Nahum-Shani 2018, sections « components » et « decision rules » (30 min).
2. Klasnja 2018, abstract + résultats + discussion (30 min).
3. Russo 2018, chapitres 1–2 (30 min).
4. Song 2025, abstract + design + résultats (20 min).
5. Doc Anthropic structured outputs (10 min).

### 1 journée
Ajouter : Haag 2025, Karine & Marlin 2025, HeartSteps RL (Liao 2019), Fogg,
Michie 2011, PAR-Q+, et parcourir le survey de Bouneffouf.

### 1 semaine
Ajouter : Lattimore & Szepesvári (chapitres choisis), GAMBITTS, Alamdari 2024,
Open Bandit Pipeline, Eysenbach 2005, Mehrotra 2016, et les références de sécurité.

---

## 11. Ce que la littérature ne nous dira pas

À garder en tête pour ne pas sur-promettre :

1. **La récompense.** Aucun papier ne dit que « pause de 2 min » est le bon
   outcome. HeartSteps montre même que les suggestions anti-sédentarité n'ont pas
   bougé les pas. Il faudra **définir et assumer** notre proxy.
2. **Le catalogue d'actions.** La littérature donne des catégories (marche,
   renforcement, mobilité, respiration, social), pas 20 actions prêtes à l'emploi.
3. **Le bon moment en contexte français de bureau.** Les études sont américaines,
   sur des populations volontaires, avec des incitations financières. Transposer
   demande de l'humilité.
4. **Le coût à 1,1 M de membres.** Latence, tokens, cache, fallback : c'est de
   l'ingénierie, pas de la littérature.
5. **La preuve d'efficacité.** Un hackathon démontre une **mécanique**, pas un
   effet santé. Le pitch doit dire « voici comment on mesurerait l'effet », pas
   « voici l'effet ».

---

## 12. Ce que ça implique pour notre design (à trancher)

1. **Variante B, avec une nuance.** Le bandit choisit la **catégorie** et le
   **créneau** ; le LLM choisit la **formulation** dans un catalogue fermé. C'est
   ce que font Song 2025 et Karine & Marlin 2025, et c'est ce qui est démontrable
   en 12 h.
2. **Le LLM n'arbitre jamais l'exploration.** Il classe les refus, reformule, et
   éventuellement propose un patch de préférence. La politique reste bayésienne.
3. **Récompense = action complétée + effort déclaré**, pas clic ni ouverture.
   Sinon on optimise l'engagement, pas la santé.
4. **Filtre de disponibilité avant la politique** : agenda, déjà en mouvement,
   conduite, mode ne pas déranger, douleur déclarée. HeartSteps fait exactement ça.
5. **Logguer la propension** de chaque action proposée. Sans ça, pas d'évaluation
   offline crédible, et pas de réponse à « comment savez-vous que ça marche ? ».
6. **Gérer l'habituation dès le jour 1** : plafond de fréquence, rotation des
   formulations, retrait des bras saturés, mesure de la lassitude. C'est le
   résultat le plus inconfortable de HeartSteps, et donc le plus différenciant.
7. **Cold start** : 3 questions d'onboarding + priors LLM (Alamdari 2024), puis
   écrasement par les vrais retours.
8. **Refus** : séparer « créneau » et « type ». 3 refus consécutifs sur un créneau
   avec la même raison → cooldown 14 jours, puis ré-exploration à 10 %. « Pas
   envie » / « trop dur » → baisse d'intensité ou changement de catégorie, pas de
   suppression de créneau.
9. **Sécurité déterministe** : PAR-Q+ simplifié, catalogue taggé (impact, matériel,
   contre-indications), douleur → stop. Le LLM ne voit jamais une action non
   validée.
10. **Démo** : 30 jours simulés, une courbe de regret, un refus respecté, un
    créneau supprimé puis ré-exploré, et un patch de profil expliqué. C'est plus
    convaincant qu'un beau message généré.

---

## 13. Index des liens

| Réf. | Lien |
|---|---|
| Nahum-Shani 2018, JITAI | https://pmc.ncbi.nlm.nih.gov/articles/PMC5364076/ |
| Klasnja 2018, HeartSteps MRT | https://pmc.ncbi.nlm.nih.gov/articles/PMC6401341/ |
| Klasnja 2015, MRT | https://pmc.ncbi.nlm.nih.gov/articles/PMC4732571/ |
| Liao 2019, Personalized HeartSteps | https://arxiv.org/abs/1909.03539 |
| Russo 2018, Thompson Sampling | https://arxiv.org/abs/1707.02038 |
| Lattimore & Szepesvári | https://tor-lattimore.com/downloads/book/book.pdf |
| Chapelle & Li 2011 | https://papers.nips.cc/paper/2011/hash/e53a0a2978c28872a4505bdb51db06dc-Abstract.html |
| Li 2010, LinUCB | https://arxiv.org/abs/1003.0146 |
| Sutton & Barto | http://incompleteideas.net/book/the-book-2nd.html |
| Haag 2025, The Last JITAI? | https://arxiv.org/abs/2402.08658 |
| Karine & Marlin 2025 | https://arxiv.org/abs/2501.06980 |
| Song 2025, cMAB × LLM | https://arxiv.org/abs/2506.07275 |
| Brooks 2025, GAMBITTS | https://arxiv.org/abs/2505.16311 |
| Alamdari 2024, LLM priors | https://arxiv.org/abs/2406.19317 |
| Bouneffouf & Feraud 2025, survey | https://arxiv.org/abs/2505.13355 |
| Awesome LLM-Bandit | https://github.com/bucky1119/Awesome-LLM-Bandit-Interaction |
| Anthropic structured outputs | https://platform.claude.com/docs/en/build-with-claude/structured-outputs |
| Anthropic tool use | https://platform.claude.com/docs/en/agents-and-tools/tool-use/overview |
| OpenAI structured outputs | https://platform.openai.com/docs/guides/structured-outputs |
| JSON Schema | https://json-schema.org/understanding-json-schema/ |
| RFC 6902, JSON Patch | https://datatracker.ietf.org/doc/html/rfc6902 |
| RFC 7396, Merge Patch | https://datatracker.ietf.org/doc/html/rfc7396 |
| Event sourcing (Fowler) | https://martinfowler.com/eaaDev/EventSourcing.html |
| Open Bandit Pipeline | https://zr-obp.readthedocs.io/en/latest/ |
| OBP paper | https://arxiv.org/abs/2008.07146 |
| River, bandits | https://riverml.xyz/latest/api/bandit/ |
| Vowpal Wabbit | https://vowpalwabbit.org |
| MabWiser | https://github.com/fidelity/mabwiser |
| Fogg Behavior Model | https://behaviormodel.org/ |
| Michie 2011, COM-B | https://link.springer.com/article/10.1186/1748-5908-6-42 |
| Ryan & Deci 2000, SDT | https://doi.org/10.1037/0003-066X.55.1.68 |
| Lally 2010, habitudes | https://onlinelibrary.wiley.com/doi/10.1002/ejsp.674 |
| Wood & Neal 2007 | https://doi.org/10.1037/0033-295X.114.4.843 |
| Gollwitzer 1999 | https://doi.org/10.1037/0003-066X.54.7.493 |
| Eysenbach 2005, attrition | https://www.jmir.org/2005/1/e11/ |
| Mehrotra 2016, réceptivité | https://doi.org/10.1145/2858036.2858566 |
| Pielot 2017, moments opportuns | https://pielot.org/pubs/Pielot2017-UbiComp-Engagement.pdf |
| PAR-Q+ / ePARmed-X+ | https://eparmedx.com/ |
| OMS 2020, activité physique | https://www.who.int/publications/i/item/9789240015128 |
| ACSM, pré-participation | https://www.acsm.org/education-resources/trending-topics-resources/exercise-pre-participation-health-screening |
| OWASP, prompt injection | https://genai.owasp.org/llmrisk/llm01-prompt-injection/ |
