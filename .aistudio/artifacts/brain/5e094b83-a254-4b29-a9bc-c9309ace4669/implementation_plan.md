# Trackers Ultimes Budget (Ultimate Budget Tracker)

Application web de gestion budgétaire personnelle et familiale complète, alliant la méthodologie 50/30/20, les enveloppes virtuelles, le suivi d'épargne par projets et une synchronisation bidirectionnelle native avec Google Sheets via l'API Google Workspace.

---

## User Review & Critical Decisions

> [!IMPORTANT]
> Les choix fondamentaux ont été clarifiés en Phase 1. Voici la synthèse des décisions validées et la prochaine étape de sécurité :

- **Méthodologie budgétaire validée** : Architecture modulaire combinant la **règle 50/30/20** (50% Besoins essentiels, 30% Envies & loisirs, 20% Épargne & investissements) et le système d'**enveloppes virtuelles dédiées** avec plafonds mensuels et alertes de dépassement.
- **Intégration Google Sheets validée** : Synchronisation bidirectionnelle et génération automatisée d'un classeur complet sur le Google Drive de l'utilisateur (onglets *Tableau de bord*, *Transactions*, *Enveloppes*, *Épargne & Objectifs*).
- **Profil d'usage cible validé** : Gestion de budget personnel et de ménage, intégrant les flux de revenus récurrents, dépenses fixes, dépenses variables quotidiennes et tirelires projets.
- **Autorisation Workspace (Étape suivante)** : Dès validation de ce plan, le flux OAuth Google Workspace officiel sera initialisé avec les permissions minimales requises (`https://www.googleapis.com/auth/spreadsheets` et `https://www.googleapis.com/auth/drive.file`).

---

## 1. Overview & Core Concept

### Ce que fait l'application
**Trackers Ultimes Budget** transforme la gestion financière quotidienne en une expérience fluide, visuelle et sous contrôle. L'utilisateur dispose à la fois :
1. D'une interface web moderne, réactive et ergonomique pour saisir, ventiler, filtrer et analyser ses dépenses en temps réel.
2. D'un classeur Google Sheets miroir hébergé sur son propre compte Google, mis à jour automatiquement ou à la demande, garantissant la propriété totale de ses données sans enfermement propriétaire.

### Public cible & Personas
- **Ménages et couples** : Répartition équitable des charges du foyer, visualisation des postes de dépenses prioritaires et anticipation des factures saisonnières.
- **Particuliers soucieux d'épargner** : Volonté de se constituer un fonds d'urgence ou de financer des projets de vie (vacances, apport immobilier, travaux) grâce aux enveloppes d'épargne.

### Proposition de valeur
- **Zéro friction** : Saisie rapide des transactions avec calcul instantané de l'impact sur les enveloppes et sur la répartition 50/30/20.
- **Souveraineté des données** : Les données vivent dans le navigateur (cache haute performance) et sur le Google Drive de l'utilisateur. Aucune base de données externe opaque.
- **Contrôle et prévoyance** : Indicateurs de reste à vivre quotidien, cadence d'épargne et alertes préventives avant dépassement des enveloppes.

---

## 2. User Experience & Visual Design

### Parcours utilisateur clés

```
┌────────────────────────────────────────────────────────────────────────┐
│                        PARCOURS PRINCIPAL                              │
├────────────────────────────────────────────────────────────────────────┤
│ 1. Accueil & Sélection Mode ──> 2. Tableau de Bord 50/30/20            │
│    (Autonome local ou Google)       │ Reste à vivre & alertes          │
│                                     ▼                                  │
│ 4. Suivi d'Épargne & Projets ◄── 3. Enveloppes & Catégories            │
│    Objectifs & fonds d'urgence      Jauges de consommation en direct   │
│                                     │                                  │
│                                     ▼                                  │
│ 5. Journal des Transactions  ──> 6. Centre de Synchronisation Google   │
│    Recherche, filtres & saisie       Création du classeur & push/pull  │
└────────────────────────────────────────────────────────────────────────┘
```

1. **Tableau de bord de synthèse (Vue d'ensemble)** :
   - Cartes KPI synthétiques : *Revenu net mensuel*, *Dépenses engagées*, *Épargne allouée*, *Reste à vivre*.
   - Jauge 50/30/20 interactive montrant la déviation réelle par rapport à la règle théorique.
   - Graphique de flux mensuel (SVG réactif) et répartition par groupe d'enveloppes.
2. **Gestionnaire d'Enveloppes Virtuelles** :
   - Cartes d'enveloppes (Alimentation, Logement, Transports, Loisirs, Shopping, Santé, Imprévus).
   - Jauge de consommation avec couleur dynamique (vert $\rightarrow$ ambre $\rightarrow$ rouge).
   - Ajustement direct des plafonds mensuels et transfert de solde entre enveloppes.
3. **Journal & Saisie des Transactions** :
   - Tableau dense de transactions avec colonnes triables : *Date*, *Libellé*, *Enveloppe*, *Type (Besoin/Envie/Épargne)*, *Mode de paiement*, *Montant*.
   - Modal de saisie express avec catégorisation automatique et validation des montants.
   - Filtres client instantanés (recherche texte, plage de dates, groupe 50/30/20, enveloppe).
4. **Tirelires & Objectifs d'Épargne** :
   - Suivi d'objectifs (Fonds d'urgence 3 mois, Vacances, Apport, Remplacement auto).
   - Calculateur d'échéance : calcul automatique de l'effort mensuel pour atteindre la cible à date.
5. **Hub Google Sheets** :
   - Bouton de connexion Google officiel.
   - Création en un clic du classeur modèle formaté avec en-têtes et formules.
   - Synchronisation bidirectionnelle (Importer depuis Google Sheets / Exporter vers Google Sheets).
   - Journal des dernières synchronisations et lien direct d'ouverture du document Google Sheets.

### Direction Artistique & Thème Visuel
- **Style esthétique** : Dashboard financier moderne, net et professionnel, inspiré des meilleures applications de gestion de trésorerie (Linear / Stripe / Mercury).
- **Discipline anti-slop** :
  - **Zéro pill statique** : les métadonnées (catégories, dates, statuts) sont typographiées sans capsules ni bordures multicolores artificielles, avec séparateurs discrets (`·` ou `/`).
  - **Chiffres tabulaires stricts** : toutes les devises et pourcentages utilisent `tabular-nums` et police monospace afin d'assurer un alignement vertical parfait des centimes.
  - **Pas de fausses métriques** : pas de faux scores de santé financière arbitraires ni de badges encombrants dans les en-têtes.
- **Palette de couleurs (Règle 60-30-10)** :
  - *60% Toile neutre* : Arrière-plan slate épuré (`bg-slate-50 dark:bg-slate-950`).
  - *30% Surfaces structurelles* : Cartes avec contour hairline 1px discret (`border-slate-200 dark:border-slate-800`), fond blanc pur (`bg-white dark:bg-slate-900`).
  - *10% Accents fonctionnels ciblés* :
    - Émeraude (`text-emerald-600`, `bg-emerald-500`) : Revenus, soldes positifs, épargne conforme.
    - Indigo (`text-indigo-600`) : Catégories "Besoins" (50%).
    - Violet (`text-violet-600`) : Catégories "Envies" (30%).
    - Ambre (`text-amber-500`) : Catégories "Épargne" (20%) et alertes 80% de l'enveloppe.
    - Rose / Rouge (`text-rose-600`) : Dépassements de budget et débits.
- **Typographie** :
  - Titres & navigation : *Plus Jakarta Sans* / *Outfit* pour une identité forte et lisible.
  - Texte courant : *DM Sans* pour une excellente lisibilité à 13-15px.
  - Données financières & horodatages : *JetBrains Mono* / `tabular-nums`.

---

## 3. Key Product Decisions & Trade-Offs

### Décision 1 : Stockage hybride (Navigateur + Google Drive)
- **Approche retenue** : Mémorisation locale instantanée (localStorage + State React) couplée à la synchronisation à la demande ou automatique vers Google Sheets.
- **Pourquoi** : L'utilisateur peut utiliser l'outil immédiatement même hors ligne ou avant connexion Google. La synchronisation Sheets offre la sauvegarde externe sécurisée et la consultation multi-écrans sans nécessiter de serveur backend propriétaire lourd.
- **Alternative écartée** : Base de données SQL hébergée avec abonnement mensuel (inappropriée pour le besoin d'indépendance de l'utilisateur avec Google Sheets).

### Décision 2 : Sécurité OAuth et Moindre Privilège (Workspace Integration Skill)
- **Approche retenue** : Utilisation exclusive des scopes spécifiques `https://www.googleapis.com/auth/spreadsheets` et `https://www.googleapis.com/auth/drive.file`.
- **Pourquoi** : Le scope `drive.file` limite l'accès uniquement aux fichiers créés ou ouverts par l'application, protégeant ainsi l'ensemble du reste du Drive personnel de l'utilisateur.
- **Confirmation des opérations destructrices** : Tout écrasement de feuille Google Sheets ou réinitialisation déclenche un dialogue d'avertissement modal avec récapitulatif clair.

### Décision 3 : Graphiques SVG vectoriels natifs sans librairie lourde
- **Approche retenue** : Composants graphiques SVG réactifs conçus sur mesure (anneau 50/30/20, barres de progression d'enveloppes, chronologie de trésorerie).
- **Pourquoi** : Rendu ultra-rapide (<1ms), zéro dépendance npm instable ou obsolète, accessibilité native et stylage Tailwind complet.

---

## 4. Technical Architecture & Data Strategy

### Diagramme d'architecture

```
┌────────────────────────────────────────────────────────────────────────┐
│                      NAVIGATEUR CLIENT (REACT 19)                      │
│                                                                        │
│   ┌───────────────────┐    ┌─────────────────┐    ┌────────────────┐  │
│   │ TopBar & Nav      │    │ 50/30/20 Gauge  │    │ Envelopes Grid │  │
│   └─────────┬─────────┘    └────────┬────────┘    └───────┬────────┘  │
│             │                       │                     │           │
│   ┌─────────▼───────────────────────▼─────────────────────▼────────┐  │
│   │              Store Budgétaire Centralisé (Context/Hook)        │  │
│   │  - Transactions  - Enveloppes  - Objectifs  - Paramètres 50/30 │  │
│   └────────┬──────────────────────────────────────────────┬────────┘  │
│            │                                              │           │
│   ┌────────▼──────────────┐                      ┌────────▼────────┐  │
│   │ Local Cache (Storage) │                      │ Sync Engine     │  │
│   └───────────────────────┘                      └────────┬────────┘  │
└───────────────────────────────────────────────────────────┼───────────┘
                                                            │ Bearer Token
                                                            ▼
                      ┌──────────────────────────────────────────────┐
                      │          GOOGLE WORKSPACE API v4             │
                      │  - Google Sheets API (/spreadsheets)         │
                      │  - Google Drive API (/files - drive.file)    │
                      └──────────────────────────────────────────────┘
```

### Modèle de données TypeScript
- **Transaction** :
  ```typescript
  interface Transaction {
    id: string;
    date: string; // YYYY-MM-DD
    title: string;
    amount: number; // Positif pour revenu, négatif pour dépense
    type: 'need' | 'want' | 'savings' | 'income';
    envelopeId: string;
    paymentMethod: 'card' | 'transfer' | 'cash' | 'direct_debit';
    note?: string;
  }
  ```
- **Envelope** :
  ```typescript
  interface Envelope {
    id: string;
    name: string;
    category: 'need' | 'want' | 'savings';
    monthlyBudget: number;
    icon: string;
    color: string;
  }
  ```
- **SavingsGoal** :
  ```typescript
  interface SavingsGoal {
    id: string;
    name: string;
    targetAmount: number;
    currentAmount: number;
    deadline: string;
    icon: string;
  }
  ```
- **GoogleSheetSyncState** :
  ```typescript
  interface GoogleSheetSyncState {
    spreadsheetId: string | null;
    spreadsheetUrl: string | null;
    lastSyncedAt: string | null;
    isSyncing: boolean;
    syncError: string | null;
  }
  ```

---

## 5. Plan de Réalisation Étape par Étape

1. **Étape 1 : Configuration OAuth Google Workspace**
   - Appel de l'outil `set_up_oauth` pour autoriser l'accès à Google Sheets (`spreadsheets`, `drive.file`).
   - Présentation de la carte d'autorisation utilisateur.
2. **Étape 2 : Structure & Moteur Métier Budgétaire**
   - Mise en place du module de calcul 50/30/20, de gestion des enveloppes et des transactions.
   - Données de démonstration prêtes à l'emploi (foyer typique avec salaire, loyer, courses, abonnements, épargne).
3. **Étape 3 : Composants UI & Tableaux de Bord**
   - Barre de navigation 3 zones avec statut de synchronisation Google Sheets.
   - Jauge 50/30/20 interactive avec métriques de respect budgétaire.
   - Grille des enveloppes virtuelles avec barres de progression et alertes.
   - Journal des transactions avec tri, filtres et ajout rapide.
   - Module d'objectifs d'épargne avec projection temporelle.
4. **Étape 4 : Service d'Intégration Google Sheets**
   - Création du classeur `Ultimate Budget Tracker` structuré en 4 onglets :
     - *Résumé & Règle 50/30/20*
     - *Transactions*
     - *Enveloppes & Plafonds*
     - *Objectifs d'Épargne*
   - Exportation / Écriture des données par lots (`batchUpdate` / `values.update`).
   - Importation / Réconciliation avec le stockage local.
   - Dialogue de confirmation avant toute opération destructrice.
5. **Étape 5 : Polissage, Tests & Validation**
   - Vérification de la compatibilité responsive (Desktop, Tablette, Mobile).
   - Contrôle du build TypeScript et Tailwind CSS via `compile_applet`.
