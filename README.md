# Journal de Trading

Journal de trading autonome (HTML/CSS/JS, aucune dépendance backend), reprenant la structure de ton Google Sheet v5 : configuration du capital, résumé des performances, historique des trades, performance mensuelle, répartition par classe d'actif, et calculateur de taille de position.

Les données sont stockées dans le `localStorage` du navigateur — rien n'est envoyé à un serveur.

## Fonctionnalités "hype" ajoutées
- **Trophées** — 10 badges à débloquer (premier trade, paliers de trades, de gains cumulés, de séries de gains, facteur profit) dans un panneau dédié ; un toast célèbre chaque nouveau déblocage.
- **Titre de trader dynamique** — un surnom affiché dans le header, calculé à partir de tes vraies stats (RR moyen, win rate, séries, facteur profit) : "Le Sniper", "Le Stratège", "Le Discipliné"...
- **Record personnel** — ton meilleur mois à ce jour, avec un toast de félicitations si tu le bats.
- **Carte de performance partageable** — génère une image stylée (façon carte) avec tes stats du mois affiché dans le calendrier, téléchargeable en PNG pour partager.
- **Compteurs animés** — le P&L cumulé, le win rate et le Return % s'animent en comptant vers leur nouvelle valeur à chaque mise à jour.
- **Flash à l'ajout d'un trade** — un léger halo doré sur un trade gagnant, rouge discret sur une perte, à l'ajout ou à la clôture d'un trade.

## Corrections sur la version ChatGPT (dernière mise à jour)
Cette version part du fichier que ChatGPT avait enrichi (Trade Quality Score, Trading Intelligence Suite, RR auto, alertes d'objectifs, mode clair/sombre, comparaison d'années) et corrige ce qui était câblé mais jamais réellement branché :
- **Trade Quality Score** — la fonction de calcul existait mais n'était jamais appelée : le panneau restait bloqué sur "—" en permanence. Rattachée au rafraîchissement principal, elle se met à jour à chaque trade désormais.
- **Trading Intelligence Suite** (Dashboard/Risk Manager/Psychology/Prop Firm) — ne se rafraîchissait que toutes les 30 secondes via une minuterie, jamais au chargement ni après l'ajout d'un trade. Même correction.
- **Rappel de sauvegarde** — ne suivait que les exports CSV manuels, jamais tes vraies synchros GitHub automatiques, et affichait "GitHub connecté" même quand la sauvegarde était en retard (logique contradictoire). Il marque maintenant la sauvegarde à jour à chaque synchro GitHub réussie, avec un badge rouge visuel passé un certain délai.
- Variable CSS `--mono` manquante (utilisée par le score de qualité) — ajoutée.
- **Rapport PDF mensuel** ajouté — un bouton dédié dans le panneau Calendrier génère un vrai résumé compact du mois (stats + liste des trades), en plus de l'export "page complète" existant.

Vérifiés comme déjà fonctionnels (pas de correction nécessaire) : calcul RR automatique, alertes d'objectifs, mode clair/sombre, comparaison d'années, continuité multi-année sans limite en dur.

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

## Corrections apportées
- Les % de gain et le "Return %" se calculent maintenant à partir du capital réel **au début de l'année affichée** (et non plus toujours ton tout premier dépôt) — sinon tes % de 2027 auraient été faussés par ton solde de 2026.
- Le calculateur de position utilise ton **capital réel actuel** (toutes années confondues), pas le capital de départ initial.
- Le filtre "Mois" du tableau ne mélange plus les mois de différentes années.
- L'import CSV lit désormais l'en-tête au lieu de se fier à l'ordre des colonnes — un CSV exporté par une ancienne version du site s'importe correctement.

## Continuité année après année
Un sélecteur **"Année"** en haut à droite filtre tout le tableau de bord (stats, tableau, courbe d'équité, performance mensuelle, drawdown) sur l'année choisie. Rien à faire manuellement en fin d'année :
- Le 1er janvier, le site détecte automatiquement la nouvelle année et bascule dessus par défaut (dashboard vierge pour 2027, par exemple).
- Tes années précédentes restent intactes dans `data/trades.json` — sélectionne "2026" (ou l'année voulue) dans le menu pour les revoir à tout moment.
- L'option **"Toutes les années"** donne une vue globale cumulée, si tu veux voir ta progression sur plusieurs années d'un coup.

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
