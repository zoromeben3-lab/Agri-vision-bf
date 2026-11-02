# AGRIVISION BF – Observatoire agricole numérique

> De la collecte sur le terrain à la décision agricole, campagne après campagne.

Projet présenté au **Hackathon des Universités – 21e édition (Semaine du numérique 2026)**,
thème : *Souveraineté technologique – Accélérer la transformation digitale sécurisée et inclusive*.

**Démo en ligne :** https://zoromeben3-lab.github.io/Agri-vision-bf/
**Dossier de candidature :** [`docs/dossier-candidature.pdf`](docs/dossier-candidature.pdf)

---

## En bref

AGRIVISION BF aide les acteurs agricoles du Burkina Faso à recenser les producteurs, suivre les campagnes, contrôler la qualité des données et répartir les intrants (engrais) de façon traçable. Les agents de terrain travaillent **d’abord sans réseau** ; les données sont envoyées au serveur dès que la connexion revient, contrôlées, puis consolidées dans des tableaux de bord par territoire.

Ce dépôt contient le **prototype de démonstration (MVP)**. Il illustre le fonctionnement prévu, mais ce n’est pas encore la plateforme finale (voir « Ce que ce prototype est, et n’est pas »).

## Lancer la démo

Aucune installation.

- **En ligne :** ouvrir le lien de démo ci-dessus.
- **En local :** télécharger le dépôt et ouvrir `index.html` dans un navigateur récent (Chrome, Edge, Firefox).

Les polices (Google Fonts) et la bibliothèque de QR code (cdnjs) sont chargées depuis Internet. Sans connexion, l’application fonctionne avec des polices système et les QR codes sont remplacés par l’identifiant du lot.

### Comptes de démonstration

Mot de passe commun : `Faso2026`

| Identifiant | Rôle | Périmètre |
|---|---|---|
| `agent.sawadogo` | Agent de terrain | Commune de Koumbia |
| `agent.traore` | Agent de terrain | Commune de Karangasso-Vigué |
| `resp.hautsbassins` | Responsable régional | Région Hauts-Bassins |
| `national` | Cellule nationale | Vue agrégée |

## Parcours de démonstration (environ 5 minutes)

1. **Collecte hors ligne.** Se connecter en `agent.sawadogo`. Le terminal démarre *hors-ligne*. Onglet **Collecte** : saisir un producteur, ou cliquer sur **Recenser 100 producteurs (démo)**. Le compteur « en attente » augmente : tout est conservé sur le terminal.
2. **Retour du réseau.** Cliquer sur le bouton réseau en haut à droite : la connexion est rétablie et la **synchronisation** se lance. Le « serveur » valide les enregistrements (ou les rejette s’ils sont invalides).
3. **Résultats de campagne.** Onglet **Campagne et lots** : **Saisir les résultats (démo)**, puis **Synchroniser**.
4. **Contrôle qualité.** Cliquer sur **Injecter une donnée incohérente (démo)**, puis **Synchroniser** : une alerte est créée. Dans l’onglet **Collecte**, **Simuler une entente sur les besoins (démo)** puis synchroniser gonfle des besoins en engrais et déclenche d’autres alertes.
5. **Vérification humaine.** Se déconnecter, puis se connecter en `resp.hautsbassins`. Onglet **Alertes** : demander une justification. L’agent répond depuis son écran ; le responsable accepte ou demande une correction.
6. **Allocation d’engrais.** Onglet **Allocation d’engrais** : saisir un stock inférieur au besoin (par exemple 30 000 kg). Le stock est réparti **au prorata** ; les besoins jugés excessifs sont mis en attente jusqu’à vérification.
7. **Distribution traçable.** Retour en agent, onglet **Distribution** : l’engrais n’est remis que si le **code personnel du producteur** est saisi (en démo, ce code est affiché dans la dernière colonne ; en réel, l’agent ne le verrait pas).
8. **Vue d’ensemble.** Se connecter en `national` : vue agrégée. Les régions marquées *Simulée* sont fictives ; seule la région Hauts-Bassins reflète les données saisies pendant la démo. L’onglet **Historique** montre toutes les opérations.

## Ce que le prototype fait déjà

- **Trois rôles** (agent, responsable régional, cellule nationale) ; un agent ne voit que sa zone.
- **Connexion journalisée**, avec verrouillage de 30 secondes après trois échecs.
- **Recensement** des producteurs : village, culture, superficie prévue, besoin en engrais, superficie à labourer.
- **Mode hors ligne simulé**, file d’attente visible et **synchronisation** avec validation par un « serveur » simulé.
- **Contrôles de cohérence** : doublon probable (même nom, même zone), besoin en engrais supérieur à 120 % d’un plafond indicatif, superficie réelle très supérieure à la superficie prévue, rendement hors plage habituelle.
- **Circuit d’alerte** avec vérification humaine : à vérifier, justification demandée, justifiée, puis vérifiée ou à corriger.
- **Résultats de campagne** : superficie réelle, production, difficulté rencontrée.
- **Allocation d’engrais au prorata** et **distribution confirmée par le producteur** (code personnel).
- **Indicateur de contrôle par agent** (« à auditer ») pour orienter les vérifications, pas pour accuser.
- **Tableaux de bord** régional et national, répartition province, commune, village, fiche producteur avec son historique.
- **Historique des opérations** (qui, quand, quoi).
- **Module expérimental** de lots avec QR code et ventes simulées (voir la note plus bas).

## Ce que ce prototype est, et n’est pas

Par souci de transparence, voici les limites actuelles :

| Sujet | État actuel du prototype | Cible décrite dans le dossier |
|---|---|---|
| Serveur et base de données | **Aucun.** Tout est stocké dans le `localStorage` du navigateur ; le « serveur » est simulé dans la même base locale. | API Node.js et PostgreSQL |
| Mode hors ligne | **Simulé** par un bouton ; pas de service worker, l’application doit avoir été chargée une fois. | PWA avec stockage IndexedDB persistant |
| Authentification | **Côté navigateur uniquement**, comptes et mots de passe de démonstration visibles. Aucune sécurité réelle. | Authentification serveur, hachage robuste, HTTPS, droits vérifiés côté serveur |
| Campagnes | Une seule campagne. | Chaque fiche rattachée à une campagne, comparaisons multi-campagnes |
| Identité du producteur | Identifiant aléatoire. | Identifiant unique durable et détection des doublons |
| Consentement, GPS, export | Non implémentés. | Consentement enregistré, GPS facultatif, export CSV et Excel |
| Référentiel territorial | **Villages fictifs.** | Référentiel officiel |
| Plafonds d’engrais et plages de rendement | **Valeurs indicatives.** | Références agronomiques officielles, validées par les services agricoles |
| Vue nationale | Quatre régions **simulées** (étiquetées comme telles). | Données réelles par région |

Les lots avec QR code et les ventes simulées relèvent de la traçabilité commerciale : ils sont présentés ici comme **module expérimental** et ne font pas partie du périmètre du premier pilote.

## Feuille de route

- [x] Prototype navigable : rôles, collecte, synchronisation simulée, alertes, allocation, distribution
- [ ] Campagne agricole comme dimension et **comparaison de deux campagnes**
- [ ] Consentement du producteur enregistré dans la fiche
- [ ] Export CSV et Excel
- [ ] PWA : service worker et stockage IndexedDB persistant
- [ ] API Node.js et base PostgreSQL, authentification côté serveur
- [ ] Référentiel territorial officiel et références agronomiques validées
- [ ] Identifiant unique durable du producteur et fusion des doublons
- [ ] Pilote terrain dans une commune (voir le dossier de candidature)

Cases cochées : fait. Cases vides : prévu, pas encore réalisé.

## Données et vie privée

Ce dépôt ne contient **aucune donnée réelle**. Les noms, villages et chiffres sont fictifs ou générés pour la démonstration. Ne jamais publier de vraies données de producteurs dans un dépôt public.

## Équipe

- **Zorome Ben Abdoul Kabir** – coordination, conception et architecture
- **Ouattara Oula Massa Souleymane** – développement, intégration et tests

## Droits d’utilisation

Aucune licence n’est encore attachée à ce dépôt : tous droits réservés par défaut. Le choix d’une licence sera fait avec les partenaires institutionnels.
