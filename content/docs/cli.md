---
title: "Utilisation de la CLI"
weight: 3
---

### Utilisation de la CLI

La ligne de commande (**CLI**) de PulseGuard permet d’interagir avec le gestionnaire de mots de passe directement depuis le terminal, sans interface graphique.  
Elle est construite sur le module Python `argparse` et repose sur une architecture **data-driven** décrite dans le fichier `commands.py`.

---

### Structure technique

#### 1. Entrée principale

L’entrée principale du programme est la fonction `main()` dans `cli.py`.  
Elle gère deux modes d’exécution :
- **Mode CLI direct** : lorsqu’une commande est passée (`pulseguard add ...`),
- **Mode console interactive** : lorsqu’aucune commande n’est spécifiée (`pulseguard` seul).

```bash
pulseguard get Gmail     # Mode CLI
pulseguard               # Lance la console interactive
```

---

#### 2. Système de commandes dynamique

Les commandes sont **définies sous forme de données** dans `commands.py` à l’aide de la classe `Command`.  
Chaque commande y est décrite avec :
- son **nom** et ses **alias** (`delete`, `del`, `rm`, etc.),
- une **description**,
- un **exemple d’utilisation**,
- un **handler** (fonction Python exécutée),
- une liste d’**arguments** et de leurs types (`str`, `int`, `bool`, etc.).

Ce modèle rend la CLI **extensible** :  
ajouter une nouvelle commande ne nécessite que de l’enregistrer dans cette liste.

---

#### 3. Génération automatique du parser

Le fichier `cli.py` construit dynamiquement un `argparse.ArgumentParser` à partir des définitions contenues dans `commands.py`.  
Ainsi, toutes les options (`--url`, `--gen`, `--length`, etc.) sont ajoutées automatiquement sans duplication de code.

Cela permet aussi de générer dynamiquement le texte d’aide accessible avec :

```bash
pulseguard --help
```

---

#### 4. Gestion du coffre (Vault)

Lors de l’exécution d’une commande :
- PulseGuard vérifie la présence du fichier de coffre (`vault.json`) ;
- s’il n’existe pas, un **nouveau coffre chiffré** est créé après saisie d’un mot de passe maître ;
- sinon, il demande ce mot de passe pour **déverrouiller** le coffre.

Cette étape est gérée par la fonction `initialize_vault()`, qui s’assure que toutes les commandes manipulent un coffre sécurisé via la classe `Vault`.

---

#### 5. Exécution des commandes

Chaque commande est associée à un **handler** défini dans `operations.py`.  
Le module `cli.py` :
1. identifie la commande saisie,
2. construit dynamiquement la liste d’arguments,
3. appelle le handler correspondant.

Exemple de logique :
```python
handler = get_command_handler(args.command)
handler(vault, *arguments)
```

Les erreurs (comme une commande inconnue ou un mauvais usage) sont capturées et accompagnées d’un message clair.

---

#### 6. Console interactive

En l’absence de commande explicite, PulseGuard lance une console (`console.py`) basée sur la bibliothèque standard `cmd`.  
Elle offre :
- une **boucle interactive** avec autocomplétion,
- la **même logique de commandes** que la CLI (`add`, `get`, `search`, etc.),
- des **alias** simplifiant la saisie (`ls`, `g`, `d`, etc.).

Pour quitter la console :
```bash
quit
# ou
exit
```

---

#### 7. Génération de mots de passe

Certaines commandes (`add`, `genpass`) permettent de générer des mots de passe aléatoires en contrôlant :
- la longueur (`--length`),
- la présence de majuscules/minuscules,
- de chiffres,
- et de symboles.

Ce comportement est intégré directement au parsing des arguments, garantissant la cohérence entre la CLI et la console interactive.

---

## Résumé

| Élément                      | Fichier concerné         | Rôle principal                              |
|------------------------------|--------------------------|---------------------------------------------|
| `cli.py`                     | Entrée principale CLI    | Parsing, exécution et gestion du coffre     |
| `commands.py`                | Définition des commandes | Architecture data-driven des commandes      |
| `console.py`                 | Console interactive      | Interface `cmd` pour usage en boucle        |
| `operations.py`              | Handlers d’exécution     | Actions métiers (add, get, delete...)       |
| `vault.py`                   | Gestion du coffre        | Lecture/écriture sécurisée (JSON chiffré)   |
|-------------------------------------------------------------------------------------------------------|
---