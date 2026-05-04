# ⚽ SportyPro CI — Pronostics Football

**Par Kouakou Cedric · Saison 2025/2026**

Application web Python/Flask de pronostics football avec données réelles via **football-data.org**.  
Calendrier, classements, buteurs et pronostics statistiques (modèle Double-Poisson + forme + H2H).

---

## 🏆 Compétitions disponibles

Toutes gratuites avec la clé free-tier football-data.org :

| Compétition | Code |
|-------------|------|
| Premier League | `PL` |
| La Liga | `PD` |
| Bundesliga | `BL1` |
| Serie A | `SA` |
| Ligue 1 | `FL1` |
| Champions League | `CL` |
| Eredivisie | `DED` |
| Primeira Liga | `PPL` |
| Championship | `ELC` |
| Série A Brésil | `BSA` |
| FA Cup | `FAC` |
| DFB-Pokal | `DFB` |
| Copa Libertadores | `CLI` |
| Copa Sudamericana | `CS` |
| Coupe du Monde 2026 | `WC` |
| Qualifications WC (UEFA, CAF, AFC, CONMEBOL, CONCACAF) | — |

> ⚠️ CAF/AFCON, MLS, Ligue 2 française → plan payant (€49/mois min.)

---

## 🧮 Moteur de pronostic

Les pronostics sont générés automatiquement selon les données disponibles :

- **Double-Poisson + classement** — ratings attaque/défense calculés depuis le classement de la ligue
- **+ Forme récente** — pondération exponentielle des 5 derniers matchs (coefficient DECAY = 0.78)
- **+ H2H** — facteur d'ajustement basé sur les 6 derniers duels directs
- **Cotes indicatives** — calculées depuis les probabilités avec marge de 5.5 %

Le niveau de confiance affiché reflète la quantité de données disponibles pour le calcul.

---

## 🚀 Lancer en local

```bash
# 1. Installer les dépendances
pip install -r requirements.txt

# 2. Définir les variables d'environnement (optionnel, des valeurs par défaut existent)
export FOOTBALLDATA_KEY=votre_cle
export SECRET_KEY=votre_secret

# 3. Lancer le serveur
python app.py

# 4. Ouvrir dans le navigateur
# http://localhost:5000
```

---

## 🌐 Déploiement sur Render.com (gratuit)

### 1. Pousser sur GitHub

```bash
git init
git add .
git commit -m "SportyPro CI v3.0"
git remote add origin https://github.com/TON_USERNAME/sportypro-ci.git
git branch -M main
git push -u origin main
```

### 2. Créer le service sur Render

| Paramètre | Valeur |
|-----------|--------|
| Runtime | Python 3.11 |
| Build Command | `pip install -r requirements.txt` |
| Start Command | `gunicorn app:app --bind 0.0.0.0:$PORT --workers 1 --timeout 120` |
| Instance Type | Free |

**Variables d'environnement à définir :**

| Clé | Valeur |
|-----|--------|
| `FOOTBALLDATA_KEY` | votre clé football-data.org |
| `SECRET_KEY` | chaîne aléatoire longue |

Le site sera live sur `https://sportypro-ci.onrender.com`.

### 3. Éviter la mise en veille (plan gratuit)

Créer un moniteur sur [UptimeRobot](https://uptimerobot.com) :

- **URL** : `https://sportypro-ci.onrender.com/api/status`
- **Intervalle** : 10 minutes

---

## 📁 Structure du projet

```
sportypro-ci/
├── app.py              # Application Flask (routes + logique + templates intégrés)
├── requirements.txt    # Flask, requests, gunicorn, python-dotenv
├── Procfile            # Config déploiement
├── render.yaml         # Config Render.com
└── .cache/             # Cache JSON auto-généré (TTL 2h)
```

> Les templates HTML sont intégrés directement dans `app.py` via `render_template_string`.

---

## 📡 API

**football-data.org v4** — [docs.football-data.org](https://docs.football-data.org)
- Clé gratuite sur inscription
- Limite : 10 requêtes/minute (throttle intégré dans l'app)
- Cache local 2h pour minimiser les appels

---

## 🔗 Endpoints utiles

```
/                          → Accueil — matchs du jour
/?date=2026-05-10          → Matchs d'une date précise
/api/status                → Santé de l'application (JSON)
```

---

⚠️ *Les jeux d'argent peuvent être dangereux. Jouez de façon responsable. +18 uniquement.*

---

*Développé par Kouakou Cedric — 2026*
