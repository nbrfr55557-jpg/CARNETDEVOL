# Carnet de vol

PWA autonome pour saisir, archiver et totaliser ses vols (format livret militaire). Fonctionne hors ligne, s'installe sur l'écran d'accueil, aucune donnée ne quitte le téléphone.

## Contenu du dépôt

```
index.html               application complète (HTML, CSS, JS vanilla)
sw.js                    service worker : hors ligne et mises à jour
manifest.json            installation sur l'écran d'accueil
icon-192.png, icon-512.png, icon-maskable-512.png, apple-touch-icon.png
```

## Déploiement GitHub Pages

1. Créer un dépôt et y déposer tous les fichiers à la racine.
2. Settings → Pages → Deploy from a branch → `main` → `/ (root)`.
3. Ouvrir `https://<utilisateur>.github.io/<dépôt>/` dans Safari, puis Partager → Sur l'écran d'accueil.

## Mettre à jour l'app

À chaque modification, incrémenter `CACHE` dans `sw.js` (`carnet-vol-v1` → `carnet-vol-v2`). Au lancement suivant, une bannière propose la mise à jour. Les vols ne sont pas touchés.

## Données

- Stockées dans le `localStorage` du téléphone, clé `carnetvol.data.v1`. Le dépôt ne contient que le code.
- Supprimer l'app de l'écran d'accueil efface les vols : exporter régulièrement la sauvegarde JSON (Réglages → Exporter la sauvegarde) vers Fichiers ou iCloud Drive.
- L'import JSON propose de fusionner (ajoute les vols absents, garde la version la plus récente d'un vol modifié) ou de remplacer tout le carnet.
- L'export CSV (séparateur `;`, UTF-8) s'ouvre directement dans Excel.

## Colonnes d'un vol

Les durées se saisissent en heures décimales, une décimale au plus, virgule ou point (1,8 = 1 h 48), et s'affichent partout dans ce format. Elles sont stockées en minutes.

Date, séance simulateur, appareil, numéro, fonction à bord, nature du vol, mission, autre membre d'équipage, jour dont VOLTAC, nuit dont SIL et dont VTN, dont VSV (jour ou nuit), procédures (ILS, VOR, POA, GCA, SCA par défaut), munitions tirées de jour et de nuit (canon, Hellfire, roquette, vols réels uniquement), remarques. Une séance simulateur ne comporte ni nature de vol ni munitions.

Chaque vol ou séance peut être marqué Test jour, Test VI ou Éval SIL, et comporter une séance de procédures d'urgence (PU). La carte Récence des Totaux suit :
- le dernier vol, vol de nuit, SIL, VTN, VOLTAC et VSV : ambre au-delà de 2 mois, rouge au-delà de 3 mois (de date à date) ;
- les tests annuels (vols réels uniquement, un test noté sur une séance simulateur reste visible dans la liste mais ne compte pas) et la PU en vol : valables 12 mois, ambre dans le dernier mois, rouge après l'échéance ;
- la PU simulateur : à faire dans les 6 mois qui suivent la dernière PU en vol.

Réglages → « Impression mensuelle A5 » produit, pour le mois choisi, un relevé à coller dans le carnet papier : tableau des vols puis des séances simulateur (jour dont VOLTAC, nuit dont SIL et VTN, VSV, total, observations), récapitulatif du mois et cumul de l'année, contrôles du mois et de l'année, heures sur 12 mois glissants, puis la mention « Certifié exact et conforme au registre des services aériens » et les encarts de signature du commandant d'unité et de l'intéressé. Les pages sont au format A5 ; sur une imprimante A4, chaque page s'imprime en haut de la feuille et se découpe. Depuis l'aperçu d'impression iOS, le bouton Partager permet aussi d'enregistrer un PDF.

Réglages → « Récence antérieure au carnet » permet de saisir les dates des derniers tests et PU effectués avant de tenir le carnet. Le total du vol est calculé (jour + nuit). La mission et l'autre membre d'équipage sont obligatoires pour enregistrer un vol ou une séance simulateur. Les listes d'appareils, fonctions, natures et procédures se modifient dans Réglages et s'enregistrent automatiquement.

L'onglet Vols affiche en tête les heures de vol réel des 12 derniers mois face à l'objectif (140 h par défaut, modifiable dans Réglages), avec les heures qui sortiront du compteur dans les 30 jours. Le bouton Nouveau vol propose d'abord vol réel ou simulateur ; une séance simulateur s'affiche sur fond bleu et n'entre pas dans les heures de vol.

Les vols saisis avec la version 1.0 sont repris automatiquement (le JVN devient SIL, départ et arrivée sont abandonnés).
