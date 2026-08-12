# Déploiement — page « Voicebot de crise »

Dossier statique autonome. Aucune dépendance, aucun build.
Il contient : `index.html`, `architecture.html`, `assets/`, le PDF A3,
plus `robots.txt`, `_headers` et `.htaccess` qui bloquent l'indexation.

Les deux pages portent déjà `<meta name="robots" content="noindex, nofollow, …">`.
Le PDF, lui, ne peut être protégé que par un **en-tête HTTP** — d'où `_headers`
(Cloudflare) et `.htaccess` (Hostinger). Ne pas les supprimer.

---

## Option A — Hostinger (recommandé : le site y est déjà)

Aucun changement DNS, aucun risque pour le site officiel.

1. hPanel → **Domaines → Sous-domaines** → créer `voicebot.banana-navy.ai`
2. hPanel → **Gestionnaire de fichiers** → ouvrir le dossier du sous-domaine
3. Téléverser tout le contenu de ce dossier (garder `.htaccess`, fichier caché)
4. Vérifier : `https://voicebot.banana-navy.ai`

Variante sans sous-domaine — un dossier au nom non devinable :
`public_html/vb-2026-a7f3k9/` → `https://banana-navy.ai/vb-2026-a7f3k9/`

## Option B — Cloudflare Pages + CNAME chez Hostinger

Le domaine reste chez Hostinger ; seul le sous-domaine pointe vers Cloudflare.

```
npx wrangler login
npx wrangler pages project create banana-navy-voicebot --production-branch main
npx wrangler pages deploy . --project-name banana-navy-voicebot
```

Puis :
1. Cloudflare → Pages → le projet → **Custom domains** → ajouter `voicebot.banana-navy.ai`
2. Hostinger → **DNS** → enregistrement `CNAME` :
   `voicebot` → `banana-navy-voicebot.pages.dev`
3. Attendre la validation du certificat (quelques minutes)

## Vérifier la non-indexation

```
curl -sI https://voicebot.banana-navy.ai/ | grep -i x-robots-tag
curl -sI https://voicebot.banana-navy.ai/Banana-Navy-Voicebot-de-crise-A3.pdf | grep -i x-robots-tag
```

Les deux doivent renvoyer `noindex, nofollow, …`.

## Rester invisible

- Ne poser aucun lien depuis banana-navy.ai (ni menu, ni pied de page, ni sitemap)
- Ne pas soumettre l'URL à la Search Console
- Envoyer le lien uniquement par e-mail ou message direct
