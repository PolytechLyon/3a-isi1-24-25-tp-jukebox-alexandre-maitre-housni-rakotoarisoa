# Compte Rendu TP Jukebox

## Binôme
- Nom Prénom
- Nom Prénom

# Jukebox Project

## Composants principaux

### AddTrackForm.vue
Permet d'ajouter des pistes à la playlist via une URL ou un fichier local.

### Player.vue
Gère la lecture des pistes, la progression, les modes de répétition, et la gestion des erreurs.

### Playlist.vue
Contient le formulaire d'ajout et la liste des pistes.

### App.vue
Coordonne le Player et la Playlist.

## Fonctionnalités principales

- **Ajout de pistes** : Possibilité d'ajouter des pistes à la playlist par URL ou fichier local.
- **Modes de lecture** : Gestion des modes de répétition (liste, piste unique, pas de répétition).
- **Progression** : Barre de progression cliquable pour naviguer dans une piste.
- **Gestion des erreurs** : Identification et gestion des fichiers invalides.
- **Suppression** : Possibilité de supprimer des pistes de la playlist.

## Structure des fichiers

- Les composants Vue.js sont bien segmentés par fonctionnalité.
- Les styles CSS sont ajoutés avec des portées locales pour éviter les conflits globaux.

## Améliorations possibles

- **Validation d'URL** : Ajouter une validation plus stricte des URLs pour s'assurer qu'elles pointent à des fichiers audio valides.
- **UI/UX** : Intégrer une notification ou un toast pour informer l'utilisateur des erreurs ou des actions réussies (ex. : fichier invalide, piste ajoutée).
- **Sauvegarde** : Permettre la sauvegarde de la playlist dans le stockage local pour persistance entre les sessions.

## Notes supplémentaires
Si vous souhaitez des optimisations ou des fonctionnalités supplémentaires, n'hésitez pas à le demander !
