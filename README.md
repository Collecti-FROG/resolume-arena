# Resolume Arena — Collecti'FROG 🐸

Dépôt des créations Resolume Arena du collectif : compositions, mappings de
sortie, layouts d'interface, raccourcis MIDI/OSC et effets partagés.

> ⚠️ **Ce dépôt est public.** Tout ce qui est committé est visible par
> n'importe qui. Les médias (vidéos, assets clients) n'y ont jamais leur place.

## Installation

Cloner le dépôt **dans le dossier Resolume lui-même** :

```
C:\Users\<toi>\Documents\Resolume Arena\
```

Tout est ignoré par défaut : tes compositions personnelles et celles d'autres
collectifs cohabitent dans le dossier sans jamais partir sur GitHub.

## Convention de nommage

Tout fichier appartenant au collectif est préfixé **`CF-`**. C'est ce préfixe,
et lui seul, qui décide si un fichier est versionné.

```
Compositions/CF-Totem.avc              ← versionné
Compositions/MonProjetPerso.avc        ← ignoré, reste chez toi
```

Les effets dans `Extra Effects/` ne sont pas préfixés : ce sont des outils,
pas des shows.

**Règle importante :** une créa ne modifie jamais un fichier appartenant à
`master`. Elle en fait une **copie à son nom**, et travaille sur la copie.

```
cp Compositions/CF-Totem.avc Compositions/CF-MaPresta.avc
```

Copier plutôt que renommer : la branche n'ajoute que des chemins qui
n'existent nulle part ailleurs, donc `git merge master` reste trivial pendant
toute la prépa. Un renommage, lui, s'approprie un chemin que `master` possède
aussi et finit en conflit.

Tout le template ne se copie pas pour autant :

| Fichier | Dépend de | À copier ? |
|---|---|---|
| La composition `.avc` | le show | **oui** |
| `Presets/Advanced Output/` | le lieu (écrans, cadrage) | **oui** |
| `Shortcuts/MIDI/`, `Shortcuts/OSC/` | le contrôleur | non, partagé |
| `Presets/Interface/` | l'écran, les habitudes de l'opérateur | non, partagé |

Ce qui reste partagé continue d'hériter des améliorations du template. Le jour
où il faut en modifier un, on le copie à ce moment-là.

## Modèle de branches

| Branche | Contenu | Devenir |
|---|---|---|
| `master` | Templates prêts à jouer : mappings, layouts, surfaces de contrôle | permanent |
| `show/Nom` | Préparation d'un nouveau template | mergée dans `master`, puis supprimée |
| `crea/Nom` | Création artistique d'une presta précise | jamais mergée, gelée + taguée après la presta |

Les flux :

- `master` → `crea/Nom` : **oui**, autant que nécessaire tant que la créa est
  active. C'est comme ça qu'elle hérite des corrections de template.
- `crea/Nom` → `master` : **jamais en merge.** Si une créa produit quelque
  chose de réutilisable, on l'extrait, on le renomme `CF-*` et on le commite
  sur `master` dans un commit dédié.

Après une presta, on tague et on gèle :

```
git tag presta/FdlM2027-2027-06-14
git push origin presta/FdlM2027-2027-06-14
```

## Médias

**Aucun média n'est versionné**, hors quelques assets légers du collectif dans
`Media/CF-Commun/` (logo, textures).

```
Media/
├── CF-Commun/     ← versionné : logo du collectif, quelques Ko
└── assets/        ← JAMAIS versionné
    ├── loops/     ← bibliothèque partagée du collectif (Nextcloud)
    └── clients/   ← assets d'autres collectifs, ne quittent pas ta machine
```

La bibliothèque partagée est sur le Nextcloud du collectif. **Le lien n'est pas
dans ce dépôt** puisqu'il est public : demande-le à un membre, puis décompresse
dans `Media/assets/loops/`.

Les assets appartenant à d'autres collectifs restent dans `Media/assets/clients/`
et ne sont ni committés, ni déposés sur le Nextcloud partagé.

### Médias manquants à l'ouverture

Resolume stocke des chemins **absolus**. Une composition ouverte sur une autre
machine signalera donc ses clips en « media offline » : il faut les re-linker
une fois. Garder la même arborescence sous `Media/assets/` chez tout le monde
rend l'opération beaucoup plus rapide.

## Effets supplémentaires

Certaines compositions utilisent des effets faits avec Wire, dans
`Extra Effects/`. Pour les installer, il suffit de lancer le fichier `.wired`.

## Avant de committer

- Le fichier est-il préfixé `CF-` ? Sinon il ne partira pas.
- Est-ce bien la bonne branche ? Un fichier de créa n'a rien à faire sur `master`.
- `git status` ne doit jamais montrer de `.mov` ou de `.mp4`.
