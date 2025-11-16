---
title: "Utilisation de la CLI"
weight: 3
---

## Introduction

La CLI de **PulseGuard** permet de gérer vos mots de passe directement
depuis le terminal.\
Elle repose sur un système de commandes structurées, chacune disposant
d'un usage clair et d'arguments bien définis.

L'accès à la CLI peut se faire de deux manières :

-   **Mode direct** : en exécutant une commande PulseGuard dans le
    terminal\
-   **Mode console interactive** : en lançant simplement `pulseguard`

------------------------------------------------------------------------

## Commandes principales

PulseGuard prend en charge plusieurs opérations essentielles pour la
gestion de votre coffre :

| Commande | Description |
|----------|-------------|
| `list` | Lister les mots de passe enregistrés |
| `add` | Ajouter une nouvelle entrée |
| `get` | Afficher les détails d’un mot de passe |
| `edit` | Modifier une entrée de manière interactive |
| `delete` | Supprimer une entrée |
| `search` | Rechercher dans le coffre |
| `demo` | Charger des données de démonstration |

Chaque commande est accessible via :

``` bash
pulseguard <commande> [arguments]
```

------------------------------------------------------------------------

## Console interactive

Lancer la console :

``` bash
pulseguard
```

Dans ce mode, les commandes sont similaires à celles de la CLI, mais
sans préfixe `pulseguard`.\
Des alias sont également disponibles pour accélérer la saisie.

------------------------------------------------------------------------

## Exemple d'utilisation

Commandes courantes :

``` bash
pulseguard list
pulseguard add Gmail user@example.com pwd123
pulseguard get Gmail
pulseguard edit Gmail
pulseguard delete Gmail
pulseguard search gmail
pulseguard demo
```

------------------------------------------------------------------------

#### `list` - Listera les mots de passe enregistrés

**Description**

Affiche toutes les entrées enregistrées dans le coffre, avec leur nom et leur identifiant (username).

**Utilisation**

```bash
pulseguard list

#exemple utilisation:
pulseguard> list
Found 3 entry(ies):
1. Gmail - user@gmail.com
2. GitHub - developer
3. Bank - user123
```

#### `add` - Ajouter une nouvelle entrée
**Description**

Crée une nouvelle entrée dans le coffre avec un nom, un identifiant (username) et un mot de passe.
Il est possible d’ajouter en option une URL et une note.

**Utilisation**

```bash
pulseguard add <name> <username> <password> [--url URL] [--notes NOTES]

#exemple utilisation:
pulseguard> add SiteTest testuser testpass
Added entry 'SiteTest' successfully.

pulseguard> add SiteTest2 testuser testpass --url https://example.com
Added entry 'SiteTest2' successfully.

pulseguard> add SiteTest3 testuser testpass --url https://example.com --notes "reminder for your test"
Added entry 'SiteTest3' successfully.

pulseguard> add SiteTest3 testuser testpass --url https://example.com --notes "reminder for your test"
Added entry 'SiteTest3' successfully.
```

#### `get` - Afficher les détails d’un mot de passe
**Description**

Affiche tous les détails d’une entrée : nom, identifiant, mot de passe, et éventuellement URL et notes.

**Utilisation**

```bash
pulseguard get <name>

#exemple utilisation:
pulseguard> get SiteTest
Password: SiteTest
Username: testuser
Password: testpass
```

#### `get` - Afficher les détails d’un mot de passe
**Description**

Affiche tous les détails d’une entrée : nom, identifiant, mot de passe, et éventuellement URL et notes.

**Utilisation**

```bash
pulseguard get <name>

#exemple utilisation:
pulseguard> get SiteTest
Password: SiteTest
Username: testuser
Password: testpass
```

## Génération automatique de mot de passe

Lors de l'ajout d'une entrée, PulseGuard peut générer un mot de passe
pour vous grâce au flag `--gen` :

Options :

-   `--length <int>`\
-   `--symbols true|false`\
-   `--upper true|false`\
-   `--lower true|false`\
-   `--digits true|false`

Exemples :

``` bash
pulseguard add Gmail user@example.com dummy --gen
pulseguard add Gmail user@example.com dummy --gen --length 18
pulseguard add Gmail user@example.com dummy --gen --length 20 --symbols false --upper true --lower true
```

------------------------------------------------------------------------

## Remarque

Pour plus de détails sur chaque commande, exécute :

``` bash
pulseguard --help
```
