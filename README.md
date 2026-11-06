# RunRun

Site de suivi d'un programme de course à pied. Chaque personne se connecte avec son compte Google, choisit **son propre Google Sheet** (copié depuis le modèle), puis coche ses séances, note son bilan et saisit ses chiffres. Tout est écrit dans son Sheet : le site ne stocke rien.

## Fichiers

- `index.html` : le site (une seule page).
- `config.js` : les 3 identifiants Google et le lien du modèle.
- `icon.svg`, `icon-180.png`, `manifest.webmanifest` : l'icône pour l'écran d'accueil du téléphone.

## Accès Google demandés

- `drive.file` : le site n'accède **qu'au Sheet que la personne choisit** dans le sélecteur Google, jamais au reste de son Drive.
- `openid profile email` : le prénom pour l'accueil.

Ces accès sont classés « non sensibles » par Google : pas de vérification Google nécessaire.

## Configuration Google Cloud (une seule fois)

1. Va sur https://console.cloud.google.com, crée un projet nommé `RunRun`.
2. **API et services → Bibliothèque** : active **Google Sheets API**, **Google Drive API** et **Google Picker API**.
3. **API et services → Écran de consentement OAuth** (ou « Google Auth Platform ») :
   - Type d'utilisateur : **Externe**.
   - Nom de l'appli : `RunRun`, ton e-mail en contact.
   - Accès aux données : ajoute `.../auth/drive.file`, `openid`, `.../auth/userinfo.profile`, `.../auth/userinfo.email`.
   - Audience : clique **Publier l'application** (passer « En production »), sinon seules les personnes ajoutées comme testeurs pourront se connecter.
4. **API et services → Identifiants → Créer des identifiants → ID client OAuth** :
   - Type : **Application Web**.
   - Origines JavaScript autorisées : `https://TON-PSEUDO.github.io`.
   - Copie l'**ID client** → `clientId` dans `config.js`.
5. **Identifiants → Créer des identifiants → Clé API** :
   - Restreins-la : Restrictions d'application **Sites web** → `https://TON-PSEUDO.github.io/*` ; Restrictions d'API → **Google Picker API**.
   - Copie la clé → `apiKey` dans `config.js`.
6. **Tableau de bord du projet** (ou Paramètres IAM) : copie le **Numéro du projet** (que des chiffres) → `appId` dans `config.js`.

## Mise en ligne (GitHub Pages)

Dépôt public contenant ces fichiers à la racine, puis **Settings → Pages → Deploy from a branch → main / root**. L'adresse devient `https://TON-PSEUDO.github.io/NOM-DU-DEPOT/`.

## Le Sheet modèle

Le modèle doit être partagé en lecture « Tous les utilisateurs disposant du lien », pour que le lien de copie (`.../copy`) fonctionne pour tout le monde.
