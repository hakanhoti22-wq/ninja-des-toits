# Ninja des Toits

Jeu de course 3D : un chat ninja court sur les toits de la ville (HTML5 + Three.js).

- **Jouer** : https://hakanhoti22-wq.github.io/ninja-des-toits/
- **Android (APK)** : https://github.com/hakanhoti22-wq/ninja-des-toits/releases/tag/android

## Activer le classement de la famille

Le classement utilise Firebase Firestore (gratuit pour un usage familial).

1. Va sur https://console.firebase.google.com et crée un projet (par exemple `ninja-des-toits`).
2. Menu **Build › Firestore Database** › **Créer une base de données** (mode production, région Europe).
3. Onglet **Règles**, colle ces règles puis **Publier** :

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /groups/{group}/scores/{doc} {
         allow read: if true;
         allow create: if request.resource.data.keys().hasOnly(['name', 'score', 'date'])
           && request.resource.data.name is string
           && request.resource.data.name.size() > 0 && request.resource.data.name.size() <= 14
           && request.resource.data.score is int
           && request.resource.data.score >= 0 && request.resource.data.score < 100000000
           && request.resource.data.date is timestamp;
       }
     }
   }
   ```

4. **Paramètres du projet** (roue dentée) › **Général** › **Vos applications** › icône **Web** (`</>`), donne un nom, puis copie `projectId` et `apiKey`.
5. Dans `index.html`, remplis la ligne `const LEADERBOARD = { projectId: '', apiKey: '' };`.

Ensuite, dans le jeu : **Réglages › Classement de la famille**, tout le monde entre le même code (au moins 4 lettres ou chiffres) et écrit son prénom à la fin d'une partie.

La clé web Firebase n'est pas un secret : ce sont les règles ci-dessus qui protègent la base.
