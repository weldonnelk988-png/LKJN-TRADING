# Journal de Trading

Journal de trading autonome (HTML/CSS/JS, aucune dépendance backend), reprenant la structure de ton Google Sheet v5 : configuration du capital, résumé des performances, historique des trades, performance mensuelle, répartition par classe d'actif, et calculateur de taille de position.

Les données sont stockées dans le `localStorage` du navigateur — rien n'est envoyé à un serveur.

## Héberger sur GitHub Pages

1. Crée un nouveau dépôt sur GitHub (ex. `journal-trading`).
2. Ajoute ce fichier `index.html` (et ce README) à la racine du dépôt :
   ```bash
   git init
   git add index.html README.md
   git commit -m "Journal de trading v5"
   git branch -M main
   git remote add origin https://github.com/<ton-user>/journal-trading.git
   git push -u origin main
   ```
3. Dans le dépôt GitHub : **Settings → Pages → Source → Deploy from a branch → main / (root)**.
4. Ton journal sera en ligne à `https://<ton-user>.github.io/journal-trading/` après 1-2 minutes.

## Nouveautés v3 (inspirées de DT Journal)
- **Trades ouverts** — logue un trade dès l'entrée (statut "Ouvert"), ajoute des mises à jour ("pulses") pendant qu'il est en cours, puis clôture-le avec le prix de sortie, le P&L et le RR réels
- **Journal quotidien** — plan avant session, notes pendant, bilan après, plus un sélecteur d'état d'esprit ; les trades du jour s'affichent automatiquement dessous
- **Objectifs du mois** — objectif de gain mensuel, nombre max de trades/jour, perte max acceptée/jour, avec barres de progression en temps réel
- Les statistiques (win rate, P&L, etc.) ne comptent que les trades **clôturés** — un trade ouvert n'entre dans les calculs qu'une fois fermé

## Nouveautés v2
- **Courbe d'équité** (capital dans le temps)
- **Calendrier P&L** mensuel façon heatmap (vert/rouge)
- **Setup / Stratégie** et **Notes & émotions** par trade, + lien de capture d'écran optionnel
- **Filtres** par classe d'actif / mois / statut sur le tableau
- **Export / Import CSV**
- **Drawdown max** dans le résumé des performances
- **Sauvegarde persistante via GitHub** (voir ci-dessous) — remplace le localStorage comme source de vérité

## Pourquoi tes trades "disparaissaient"
Le localStorage est propre à chaque navigateur/appareil : il ne se synchronise pas entre ton téléphone et ton ordinateur, et se vide si tu changes de navigateur ou passes en navigation privée. Ce n'est pas un bug de perte de données — la sauvegarde GitHub ci-dessous corrige ça en centralisant tes trades dans un fichier du dépôt.

## Activer la sauvegarde GitHub (persistance réelle, multi-appareils)
1. Clique sur le badge **"Stockage local"** en haut à droite du site pour ouvrir le panneau de sauvegarde.
2. Crée un token sur [github.com/settings/personal-access-tokens/new](https://github.com/settings/personal-access-tokens/new) :
   - **Repository access** → "Only select repositories" → choisis `KGB-journal`
   - **Permissions** → "Repository permissions" → **Contents: Read and write**
3. Renseigne : propriétaire (`weldonnelk988-png`), dépôt (`KGB-journal`), branche (`main`), chemin (`data/trades.json`), et colle le token.
4. Clique **Connecter**. Tes trades actuels sont poussés dans `data/trades.json` sur GitHub. À chaque ajout/suppression, le site sauvegarde automatiquement (avec un léger délai).
5. Sur un autre appareil, reconnecte-toi avec les mêmes infos + le token pour charger les mêmes trades.

⚠️ Le token est stocké uniquement dans le `localStorage` de ton navigateur et envoyé directement à l'API GitHub — jamais à un serveur tiers. Utilise bien un token **fine-grained limité à ce seul dépôt**, pas un token classique avec accès à tous tes repos.

## Notes
- Le P&L (%) est calculé automatiquement à partir du solde de départ.
- Le P&L ($) et le RR réalisé s'entrent manuellement par trade (comme dans le sheet).
- Le calculateur de position utilise : `taille = (capital × risque%) / stop en pips`.
