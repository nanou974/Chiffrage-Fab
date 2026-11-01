# Chiffrage Fab

Application de devis, fiches de revient et factures pour ateliers d'impression 3D, de découpe laser et de CNC/fraisage.
Elle fonctionne dans le navigateur, sans serveur, et peut s'installer comme une application (PWA).

## Fonctionnalités

### Profils et premier lancement
- Assistant de premier lancement : langue (FR/EN), type de profil (Maker particulier, Artisan, Fab Lab), identité, logo, paramètres de calcul.
- Plusieurs profils possibles dans une même installation.
- Adresse, code postal, ville et téléphone obligatoires pour les professionnels et pour les factures ; facultatifs pour un maker particulier.
- Sans SIRET ni numéro de TVA valide, l'application n'émet que des devis et des fiches de revient (la facturation est désactivée, avec un avertissement).

### Calcul du prix
- Types de pièces : impression 3D (poids en grammes, temps), découpe laser et CNC (surface en cm², temps en heures et minutes).
- Temps saisi en deux champs, Heures et Minutes, regroupés dans un bloc encadré.
- Postes de coût pris en compte : matière, électricité (puissance machine × durée × prix du kWh), usure de la buse (3D), échecs (taux par matière), main-d'œuvre (taux horaire), emballage, livraison, marge (25 % par défaut), TVA 20 % seulement si elle est déclarée.
- Taux d'échec et d'usure par défaut selon la matière (PLA 3 %, PETG 5 %, ABS/ASA/PA/PC 15 %, TPU 10 %, CF/GF 8 %, aluminium CNC 5 %), modifiables.
- Journal des impressions optionnel : après 10 impressions, les taux peuvent être recalibrés à partir des résultats réels.

### Documents
- **Devis** (DEV-AAAA-NNNN) : chaque pièce avec son détail (matière, électricité, usure, échecs), puis main-d'œuvre, emballage, livraison, marge, HT, TVA éventuelle et TTC. Bloc « Bon pour accord » à signer par le client.
- **Fiche de revient** (FR-AAAA-NNNN) : document interne avec le détail complet des coûts. Elle peut être supprimée.
- **Facture** (FAC-AAAA-NNNN) : numérotation continue, uniquement avec un SIRET ou une TVA valide.
- Affichage dans l'application avec un bouton Imprimer, et un lien « Ouvrir dans un onglet (Ctrl+P) » pour imprimer depuis le navigateur.

### Validation
- SIRET : 14 chiffres avec contrôle de Luhn.
- Numéro de TVA français : format FR + 2 caractères + 9 chiffres, avec contrôle de clé.
- Valeurs de test (fictives) : SIRET `12345678900015`, TVA `FR32123456789`.

### Historique et export
- Liste des devis avec accès aux documents, fiches de revient et factures.
- **Export d'archive** : choix d'une période (du / au), puis téléchargement d'un fichier ZIP contenant les devis, fiches de revient et factures en PDF, classés dans trois dossiers. Les PDF sont générés dans le navigateur.

### Avis utilisateur
- Section « Votre avis » en bas de page : note sur 5 étoiles, commentaire et suggestion.
- Le bouton ouvre la messagerie avec le texte prérempli vers `bg3d81@gmail.com`. Un encadré s'affiche aussi, avec le texte à copier et un lien « Ouvrir dans Gmail ».
- Rien n'est stocké ni envoyé à un serveur.

### Interface
- Responsive : fonctionne sur PC et téléphone.
- Paramètres avec boutons « Enregistrer les modifications » et « Annuler » ; un message signale les changements non enregistrés.

## Stockage des données
- Les données sont enregistrées dans le navigateur (`localStorage`, clé `bg3d81_devis_v3`).
- Elles restent sur l'appareil et ne sont pas partagées entre plusieurs ordinateurs ou téléphones.
- Les anciennes versions utilisaient les clés `bg3d81_parametres` et `bg3d81_devis_v2` : pas de migration automatique pour l'instant.

## Fichiers
- `index.html` : toute l'application (HTML, CSS, JavaScript). Les bibliothèques jsPDF et JSZip y sont intégrées, pour fonctionner hors ligne.
- `manifest.webmanifest` : informations d'installation (nom, couleurs, icônes).
- `sw.js` : service worker. Il garde une copie de l'application pour le hors-ligne, en privilégiant toujours la version en ligne.
- `icons/` : icônes 192 et 512 px (usine à toit en dents de scie, cube bleu sarcelle).

## Mise en ligne (GitHub Pages)
1. Dans le repo : **Settings → Pages → Deploy from a branch**, branche `main`, dossier `/ (root)`.
2. L'adresse est de la forme `https://nanou974.github.io/Chiffrage-Fab/`.
3. Sur téléphone (Chrome) : ouvrir l'adresse, puis « Installer l'application » ou « Ajouter à l'écran d'accueil ».

Le HTTPS est nécessaire pour l'installation et le service worker : un simple fichier ouvert depuis le PC ne l'active pas.

## Mises à jour
- Le service worker télécharge la version en ligne en priorité. Après une mise à jour, fermer tous les onglets puis recharger avec Ctrl+F5.
- Si l'ancienne version persiste : F12 → Application → Service Workers → Unregister, puis Ctrl+F5.

## Points à décider ou à finaliser
- **Tarification** : piste retenue, une licence unique de 30 € (sans limite d'historique). Forfait mensuel et envoi d'e-mails à voir plus tard, car ils demandent un serveur.
- **Application Android** : pour le Play Store, envelopper le site hébergé (par exemple avec Bubblewrap / Trusted Web Activity), compte développeur Google (environ 25 € une fois). À faire après une mise en ligne stable.
- **Serveur** : à prévoir plus tard pour la synchronisation entre appareils et les e-mails.
- **Prix par défaut** : les prix des matières et des machines sont à renseigner par l'utilisateur (seul le PETG est proposé à 11 €/kg).
- **Export et import de profils** : pas encore prévus (utile pour transférer les données entre appareils).
- **Tests à faire sur appareil réel** : impression depuis le navigateur, export ZIP, installation sur Android, affichage du bloc Temps sur petit écran.
- **Mention « Chiffrage Fab »** : à vérifier à l'INPI et sur les moteurs de recherche avant toute diffusion commerciale.

## Crédits
Code produit avec l'assistance de Claude (Anthropic). Bibliothèques : jsPDF (MIT), JSZip (MIT / GPL).
