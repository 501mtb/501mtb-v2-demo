# 501MTB V2 — Plan du panneau d'administration

## Objectif
Permettre aux administrateurs autorisés de mettre à jour les photos, dates, liens, profils des riders et traductions sans modifier directement le HTML/CSS à chaque fois. Ce plan concerne uniquement le dépôt `501mtb/501mtb-v2-demo`. Le site de production V1 `501mtb/501mtb.github.io` ne doit pas être modifié.

## Point important : GitHub Pages est statique
Une page `admin.html` seule sur GitHub Pages ne peut pas enregistrer de manière sécurisée des modifications dans les fichiers du dépôt ou stocker des photos. Il ne faut jamais mettre un token GitHub privé, une clé secrète ou un mot de passe dans le JavaScript public.

## Architecture recommandée
- **Frontend public (GitHub Pages)** : affiche les événements, les riders, les liens et les textes.
- **Panneau admin** : formulaire de connexion, puis écrans de gestion Événements, Riders, Photos/Médias, Liens et Traductions.
- **Authentification** : Supabase Auth, avec accès limité aux comptes administrateurs autorisés.
- **Base de données** : Supabase Postgres pour les événements, profils riders, liens et traductions.
- **Photos** : Supabase Storage, avec règles d'accès adaptées.
- **Permissions** : politiques Row Level Security (RLS) pour que seuls les administrateurs connectés puissent créer, modifier ou supprimer du contenu. Les données publiques nécessaires au site peuvent être lues sans autoriser l'écriture.
- **Configuration** : identifiants publics du projet uniquement dans la configuration frontend; les clés secrètes restent côté serveur. La clé anon/public de Supabase n'est pas un mot de passe : les règles RLS doivent protéger toutes les tables et le stockage.

## Données à gérer
### Événements
Titre, date/heure, lieu, description, photo, lien d'inscription, statut (à venir/terminé), langue si nécessaire.

### Riders et leaders
Nom affiché, statut/catégorie, biographie courte, photo, liens sociaux, ordre d'affichage, visibilité.

### Traductions
Textes structurés par clé et langue : anglais (`en`), français (`fr`), malgache (`mg`). Prévoir une valeur de secours si une traduction manque.

### Liens
Libellé, URL, type (Facebook, Instagram, inscription, GPX, règlement, etc.), emplacement sur le site, langue éventuelle, visibilité.

### Médias
Fichier image, texte alternatif, catégorie, date d'ajout. Prévoir des noms de fichiers stables et des tailles optimisées pour le mobile.

## Étapes de réalisation
1. Confirmer/ouvrir un projet Supabase dédié à V2.
2. Créer les tables, le stockage et les règles RLS; tester que les visiteurs ne peuvent pas modifier les données.
3. Créer l'écran de connexion et les pages d'administration.
4. Migrer progressivement les données utiles depuis V1 vers V2, sans copier les erreurs existantes.
5. Connecter l'accueil public V2 à la base de données.
6. Tester sur téléphone : connexion, ajout/modification/suppression, téléversement photo, langues et affichage public.
7. Garder V2 en démonstration; ne toucher à V1 qu'après validation explicite et plan de migration.

## Sécurité et publication
- Aucun compte admin ni mot de passe ne doit être codé en dur dans les fichiers publics.
- Aucune clé `service_role` ne doit apparaître dans le frontend.
- Protéger les actions d'écriture avec Auth et RLS; limiter les types et tailles de fichiers image.
- Prévoir export/sauvegarde régulière des données.
- L'interface admin n'est pas considérée fonctionnelle tant que l'authentification, les autorisations et l'enregistrement persistant ne sont pas testés.

## Limite actuelle
Ceci est le plan technique. Le panneau ne sera pas réellement connecté ni sécurisé tant que le projet backend n'est pas créé et configuré. Ne pas remplacer le contenu du site V2 actuel avant que cette configuration soit prête.
