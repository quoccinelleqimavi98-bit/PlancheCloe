# CLAUDE.md — PlancheCloe

Guide de référence pour travailler sur ce dépôt (lu en priorité par Claude, et
utile à toute personne qui reprend le projet).

## C'est quoi ce projet ?

La **planche tarifaire** de Cloé Chaudron Beauty : une appli **Angular 18** qui
affiche les prestations, prix et descriptions, modifiable via un **espace admin**.
Le contenu (textes, prix, images, positions) est enregistré côté serveur par une
petite **API PHP** (voir `backend/README.md`), configurée dans
`src/app/config.ts` (`API_BASE = https://cloechaudronbeauty.com/backend/api/`).

> ⚠️ Contrairement aux deux autres dépôts (IntraCCB2 et CloeBeauty, hébergés sur
> OVH par FTP), **le front de PlancheCloe est publié sur GitHub Pages**. Seul le
> **backend PHP** de la planche vit sur OVH (dossier `backend/`).

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
3. **C'est tout.** Le workflow « Déploiement GitHub Pages » build la planche et
   la publie sur GitHub Pages tout seul.

À la main : onglet **Actions** → « Déploiement GitHub Pages » → **Run workflow**.

> Auparavant le déploiement se faisait par un script `deploy-ghpages`
> **Windows-uniquement** (PowerShell) qui buildait dans `docs/` et poussait à la
> main. Il a été remplacé par le workflow ci-dessus, qui marche partout et ne
> committe plus les fichiers buildés dans le dépôt.

### ⚙️ Réglage à faire UNE SEULE FOIS sur GitHub

Dépôt **PlancheCloe** → **Settings** → **Pages** → *Build and deployment* →
**Source = « GitHub Actions »**.

C'est tout : **aucun secret à créer** (GitHub s'authentifie tout seul pour Pages).

## 🌿 Le backend PHP (dossier `backend/`)

- Le fichier de config `backend/api/config.php` (base de données, mot de passe
  admin, clé secrète) est **ignoré par git** — modèle : `config.sample.php`.
  Voir `backend/README.md` pour l'installation serveur.
- Le backend vit sur OVH (`cloechaudronbeauty.com/backend/api/`). Toute
  modification d'un `.php` doit être remontée **par FTP manuel** : le workflow
  Pages ne déploie que le front.
- Le site Pages est en HTTPS → l'API doit l'être aussi (sinon « mixed content ») :
  `API_BASE` dans `src/app/config.ts` doit rester en `https://`.

## Pour Cloé (en clair)

Pour modifier la planche (prix, textes, photos) : utilise l'**espace admin** de
l'appli. Pour changer l'appli elle-même : **demande à Claude** ; une fois poussé
sur `main`, ça se met en ligne tout seul sur GitHub Pages. Réglage unique :
mettre la *Source* de Pages sur « GitHub Actions » (voir ci-dessus).
