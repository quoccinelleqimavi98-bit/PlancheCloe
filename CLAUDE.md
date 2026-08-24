# CLAUDE.md — PlancheCloe

Guide de référence pour travailler sur ce dépôt (lu en priorité par Claude, et
utile à toute personne qui reprend le projet).

## C'est quoi ce projet ?

La **planche tarifaire** de Cloé Chaudron Beauty : une appli **Angular 18** qui
affiche les prestations, prix et descriptions, modifiable via un **espace admin**.
Le contenu (textes, prix, images, positions) est enregistré côté serveur par une
petite **API PHP** (voir `backend/README.md`), configurée dans
`src/app/config.ts` (`API_BASE = https://cloechaudronbeauty.com/backend/api/`).

## Développement local

```bash
npm install        # une fois
npm start          # http://localhost:4200
npm run build      # build de production dans dist/planche-cloe
npm run build:prod # build de prod avec base-href relatif (comme en déploiement)
```

## 🚀 Mettre en ligne (déploiement)

**Le plus simple, et sans rien installer : passer par GitHub.**

1. Faire la modification (ou demander à Claude de la faire).
2. La faire arriver sur la branche `main`.
3. **C'est tout.** Le workflow « Déploiement FTP (OVH) » build la planche et
   l'envoie en FTP sur OVH tout seul.

À la main : onglet **Actions** → « Déploiement FTP (OVH) » → **Run workflow**.

> Auparavant le déploiement se faisait par un script `deploy-ghpages`
> **Windows-uniquement** (PowerShell) vers GitHub Pages. Il a été retiré au
> profit du workflow FTP ci-dessus, qui marche partout et publie sur
> l'hébergement OVH (le vrai site), pas sur GitHub Pages.

### ⚙️ Réglages à faire UNE SEULE FOIS sur GitHub

Dépôt **PlancheCloe** → **Settings** → **Secrets and variables** → **Actions** :

1. **Secret** `FTP_PASSWORD` = mot de passe FTP OVH. *(obligatoire)*
2. **Variable** `FTP_REMOTE_PATH` = le dossier cible sur le serveur.
   - ⚠️ **À confirmer** : la valeur par défaut est `/public_html/planche/`. Si la
     planche doit être servie ailleurs (autre sous-dossier, sous-domaine…),
     mettre le bon chemin ici. Se tromper de chemin ne fait que créer un dossier
     inutile — ça n'écrase rien d'autre (le backend est protégé).

*(Hôte `ftp.chcl8760.odns.fr` et utilisateur `chcl8760` par défaut. Pour les
changer : variables `FTP_HOST` / `FTP_USER`.)*

> ⚠️ **Le FTP ne marche PAS depuis Claude sur le web** (port 21 bloqué dans le
> cloud). Depuis Claude, on déploie via GitHub uniquement (push sur `main` ou
> « Run workflow »).

## 🌿 Le backend PHP (dossier `backend/`)

- Le fichier de config `backend/api/config.php` (base de données, mot de passe
  admin, clé secrète) est **ignoré par git** — modèle : `config.sample.php`.
  Voir `backend/README.md` pour l'installation serveur.
- Toute modification d'un `.php` du backend doit être remontée **par FTP manuel** :
  le workflow ne déploie que le front et ne touche pas au backend.
- Si le site est en HTTPS, l'API doit l'être aussi (sinon « mixed content ») :
  adapter `API_BASE` dans `src/app/config.ts`.

## Pour Cloé (en clair)

Pour modifier la planche (prix, textes, photos) : utilise l'**espace admin** de
l'appli. Pour changer l'appli elle-même : **demande à Claude** ; une fois poussé
sur `main`, ça se met en ligne tout seul. Réglage unique : ajouter le secret
`FTP_PASSWORD` (et vérifier `FTP_REMOTE_PATH`) sur GitHub.
