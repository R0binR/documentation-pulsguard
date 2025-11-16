---
title: "Installation"
weight: 2
---

### Prérequis

- **Python 3.11 ou supérieur**  
  Vérifie la version installée :
  ```bash
  python3 --version
  ```

- **uv uv 0.9.8 ou supérieur** – Gestionnaire d’environnement et de dépendances rapide  
  ```bash
  pip install uv
  ```

---

### Installation de PulseGuard

```bash
uv pip install -e .
```

---

### Configuration initiale

Lors du premier lancement, PulseGuard créera automatiquement un **coffre (vault)** à l’emplacement suivant :

```
~/.pulseguard/vault.json
```

Tu peux personnaliser cet emplacement avec la variable d’environnement :

```bash
export PULSEGUARD_VAULT_PATH="/chemin/vers/mon_vault.json"
```

---

### Vérification de l’installation

Teste que tout fonctionne :

```bash
pulseguard demo
pulseguard list
```

Puis lance la **console interactive** :

```bash
pulseguard
```

---

## Développement (optionnel)

Pour contribuer au projet ou exécuter les tests :

Clone le dépôt et installe le projet en mode édition :

```bash
git clone https://github.com/<ton-repo>/pulseguard.git
cd pulseguard
uv pip install -e .[dev,test]
```

Cela installe PulseGuard ainsi que les dépendances de développement (`ruff`, `black`, `pytest`, etc.).

```bash
# Installation des hooks de formatage automatique
./setup-hooks.sh

# Lancer les tests
uv run pytest

# Vérification du code
uv run ruff check src tests
uv run black src tests
```

---