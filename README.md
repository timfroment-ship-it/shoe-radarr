# Brocante Shoe Radar V10

V10 change le moteur de reconnaissance pour utiliser une architecture en **deux étages**.

## Étape 1 — détection
OWL-ViT analyse l'image entière et cherche seulement :
- shoe ;
- sneaker ;
- hiking shoe ;
- boot.

Les boîtes qui se chevauchent fortement sont fusionnées et les objets trop petits sont ignorés.

## Étape 2 — classification du crop
Chaque chaussure retenue est recadrée puis analysée avec CLIP.

Classes proposées :
- Salomon ;
- Merrell ;
- Hoka ;
- La Sportiva ;
- Adidas Terrex ;
- Scarpa ;
- randonnée ;
- trail ;
- sneaker classique ;
- autre chaussure.

Une marque ne déclenche pas automatiquement une alerte parce qu'elle arrive première.

V10 vérifie aussi :
- un score minimum ;
- l'écart entre le premier et le deuxième choix ;
- une confirmation sur plusieurs cycles.

L'interface écrit volontairement **“Salomon probable”** plutôt que de prétendre à une certitude.

## Réglages importants

### Sensibilité détection
Contrôle OWL-ViT.

Valeur par défaut : 0,07.

### Confiance marque minimale
Score CLIP minimal pour annoncer une marque.

Valeur par défaut : 0,34.

### Écart avec le deuxième choix
Exemple :
- Salomon 0,46
- Merrell 0,43

L'écart est trop faible, donc V10 n'alerte pas comme si Salomon était certain.

Valeur par défaut : 0,07.

### Maximum de crops
Plus le nombre est élevé, plus V10 peut analyser de chaussures simultanément, mais plus le téléphone travaille.

2 est le réglage recommandé.

## Performances
V10 charge deux modèles dans le navigateur :
- `Xenova/owlvit-base-patch32`
- `Xenova/clip-vit-base-patch32`

Le premier démarrage est donc plus lourd que V9.

L'image complète est réduite à environ 640 px de large. Les crops sont limités à environ 384 px.

## Signal
Lorsqu'une cible est confirmée :
- rectangle sur la chaussure ;
- bannière ;
- bip ;
- vibration si disponible.

Les marques ont un signal plus marqué que la simple catégorie “randonnée/trail”.

## Limite principale
CLIP reste un modèle généraliste. V10 améliore nettement la logique de reconnaissance, mais une marque peu visible peut encore être mal classée.

Le vrai saut de précision suivant serait un petit classifieur ou détecteur entraîné avec un dataset spécifique de chaussures photographiées en brocante.
