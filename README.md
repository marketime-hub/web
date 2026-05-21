# Marketime Hub — Site

Dashboard de pilotage MH v2, déployé sur [marketimehub.fr/dashboard](https://marketimehub.fr/dashboard).

## Structure

```
mhub-site/
├── index.html                  # Page racine (redirection vers /dashboard)
├── dashboard/
│   └── index.html              # Dashboard de pilotage 2026
├── netlify.toml                # Config Netlify (headers sécurité, noindex)
├── .gitignore
└── README.md
```

## Déploiement

- **Hébergement** : Netlify (compte `baptiste.casnedi@gmail.com`)
- **DNS** : Gandi → CNAME vers Netlify
- **Deploy auto** : push sur `main` déclenche le build Netlify

## Authentification

Le dashboard est protégé par un mot de passe client-side.

Le mot de passe **en clair n'est jamais stocké dans le code**. Seul son hash SHA-256 est présent dans [dashboard/index.html](dashboard/index.html).

### Changer le mot de passe

1. Choisir un nouveau mot de passe fort
2. Calculer son hash SHA-256 :
   ```bash
   python -c "import hashlib; print(hashlib.sha256('NOUVEAU_MDP'.encode()).hexdigest())"
   ```
   ou en PowerShell :
   ```powershell
   $bytes = [System.Text.Encoding]::UTF8.GetBytes('NOUVEAU_MDP')
   ($([System.Security.Cryptography.SHA256]::Create()).ComputeHash($bytes) | ForEach-Object { $_.ToString('x2') }) -join ''
   ```
3. Remplacer la valeur de `PASSWORD_HASH` dans `dashboard/index.html`
4. Commit + push

### Réinitialiser les sessions

Le storage utilise une clé versionnée (`STORAGE_KEY = 'marketime-dashboard-2026-vN'`). Bumper le `N` force le rechargement des valeurs initiales et déconnecte tous les utilisateurs.

## Mise à jour des données

Le dashboard est en saisie manuelle. Les valeurs sont persistées dans le localStorage du navigateur de chaque utilisateur.

Pour les KPI initiaux (affichés à la première visite), ils sont en dur dans le HTML — mis à jour mensuellement au moment du comité Jérôme.

Sources de données :
- `Marketime Hub/DAF/FIN/LA/LA 2026.xlsx` (référence Eva)
- `Marketime Hub/DAF/treso jour.xlsx` (solde banque)
- `Marketime Hub/DAF/projection-treso-MH.html` (point mort structure)

## Contacts

- **Owner** : Baptiste Casnedi
- **Comité mensuel** : Baptiste, Jérôme (Breizh Solutions), Jean, Eva
