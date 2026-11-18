# Guide de fabrication — clavier 60 % handwired Bluetooth

Guide progressif pour fabriquer un clavier compact de **64 touches**, dérivé du modèle « 60 Keyboard with Arrow Keys » de MakerWorld. Cette page décrit la fabrication du boîtier, l’assemblage des switches, le câblage de la matrice et la mise en service du contrôleur nRF52840 avec ZMK.

> Ce guide documente une réalisation personnelle. Vérifiez chaque mesure et chaque connexion sur votre propre matériel avant soudure ou mise sous tension. Les photos, le schéma de câblage et les fichiers 3MF seront ajoutés ultérieurement : ils ne sont pas encore fournis dans ce dépôt.

## Sommaire

- [Aperçu du projet](#aperçu-du-projet)
- [Matériel et nomenclature](#matériel-et-nomenclature)
- [Impression 3D](#impression-3d)
- [Pose des switches et stabilisateurs](#pose-des-switches-et-stabilisateurs)
- [Câblage de la matrice 5 × 14](#câblage-de-la-matrice-5--14)
- [Contrôleur et batterie](#contrôleur-et-batterie)
- [Firmware ZMK](#firmware-zmk)
- [Tests avant fermeture](#tests-avant-fermeture)
- [Dépannage](#dépannage)
- [Photos, schémas et fichiers à venir](#photos-schémas-et-fichiers-à-venir)
- [Crédits et ressources](#crédits-et-ressources)

## Aperçu du projet

Le clavier reprend le format compact avec flèches du modèle MakerWorld et reçoit un câblage manuel, un contrôleur Bluetooth **nRF52840** et une batterie LiPo de **250 mAh**. La matrice comporte **5 lignes et 14 colonnes** (70 intersections possibles) pour les **64 switches** de cette réalisation ; les six positions non utilisées doivent rester absentes de la configuration du firmware.

Le dépôt du guide contient les étapes de fabrication. La configuration et les fichiers du firmware se trouvent dans le dépôt séparé [wireless-60-handwired-keyboard](https://github.com/Freezyi420/wireless-60-handwired-keyboard). Le dépôt personnel `zmk-config` n’est pas nécessaire pour suivre ce guide.

## Matériel et nomenclature

Les liens Amazon ci-dessous doivent correspondre aux références réellement utilisées. Un seul lien exact a été retrouvé à ce jour ; les autres sont laissés à compléter plutôt que remplacés par des annonces approximatives. Vérifiez les caractéristiques, la compatibilité et la disponibilité avant achat.

| Élément | Quantité indicative | Référence / état du lien |
|---|---:|---|
| Carte contrôleur SuperMini nRF52840 | 1 | [FEETER SuperMini nRF52840 — Amazon.fr](https://www.amazon.fr/dp/B0G4L4P4B6) — référence retrouvée, disponibilité à vérifier |
| Switches mécaniques | 64 + quelques pièces de réserve | Kailh BOX White utilisés ; lien Amazon exact à ajouter |
| Diodes 1N4148 | 64 | Une par switch ; lien exact à ajouter |
| Batterie LiPo | 1 | 250 mAh ; lien et connecteur exacts à confirmer |
| Interrupteur marche/arrêt | 1 | Modèle et lien exacts à confirmer |
| Fils de câblage | selon le montage | Fils souples isolés recommandés aux débutants ; référence à ajouter |
| Keycaps | 64 | Profil et disposition compatibles avec le clavier ; lien exact à ajouter |
| Stabilisateurs | selon les grandes touches | Tailles à relever sur la disposition retenue ; lien exact à ajouter |
| Boîtier imprimé en 3D | 1 jeu | Modèle d’origine sur MakerWorld ; fichiers de la variante à venir |
| Consommables | au besoin | Étain, gaine isolante/thermorétractable et adhésif de maintien |

**Outils utiles :** imprimante 3D, fer à souder à température réglable, étain adapté à l’électronique, pince coupante, pince à dénuder, multimètre et matériel de protection pour le travail de soudure.

## Impression 3D

1. Ouvrez le [modèle MakerWorld original](https://makerworld.com/fr/models/557186-60-keyboard-with-arrow-keys) et examinez ses instructions, ses fichiers et les conditions de licence indiquées par son auteur.
2. Téléchargez les fichiers directement depuis la page de leur auteur. Ce dépôt ne redistribue pas le modèle original.
3. Contrôlez l’échelle, les tolérances des logements de switches et l’espace prévu pour le contrôleur, la batterie et l’interrupteur. Les fichiers adaptés à cette variante ne sont pas encore disponibles ici.
4. Tranchez et imprimez les pièces en respectant les recommandations du modèle et de votre imprimante. Faites d’abord un essai d’ajustement avant d’imprimer toutes les pièces.
5. Ébavurez les logements sans forcer. Vérifiez que les switches s’enclenchent et que le boîtier peut être fermé sans comprimer les fils ou la batterie.

**Réglages et matériaux de l’impression de cette réalisation :** à documenter avec les photos et les fichiers 3MF.

## Pose des switches et stabilisateurs

1. Repérez la face supérieure du plateau et l’orientation prévue pour les switches.
2. Insérez les switches un par un, en contrôlant leur enclenchement et leur alignement. Ne forcez pas si un logement semble trop serré.
3. Installez les stabilisateurs aux emplacements prévus par la disposition, avant de souder les switches concernés si leur accès l’exige.
4. Posez provisoirement les keycaps des grandes touches et vérifiez leur course : elles ne doivent ni accrocher ni rester enfoncées.
5. Maintenez les switches en place pendant le câblage et vérifiez régulièrement que le plateau reste plan.

Les dimensions exactes des stabilisateurs dépendent de la disposition finale et seront précisées avec les photos ou le schéma.

## Câblage de la matrice 5 × 14

La réalisation utilise une matrice **5 lignes × 14 colonnes**, **64 switches** et **64 diodes 1N4148**, une diode par touche. Le paramètre de direction dans ZMK est **`col2row`** : le brochage logiciel doit correspondre au câblage physique.

### Préparation

- Dessinez ou annotez le plan de matrice avant de souder. N’inférez pas l’ordre des broches depuis la forme du boîtier : relevez les broches réellement utilisées dans la configuration ZMK et dans la documentation de votre contrôleur.
- Numérotez les lignes et colonnes de manière cohérente avec le futur schéma et la configuration firmware.
- Laissez les six intersections non utilisées sans switch et marquez-les comme telles dans le plan de matrice.
- Pour un câblage plus accessible et moins exposé aux courts-circuits, utilisez des **fils souples isolés** et contrôlez l’isolation aux croisements.

### Montage électrique

1. Reliez un côté de chaque switch à la ligne correspondante, suivant le plan de matrice.
2. Placez une diode 1N4148 en série avec chaque switch, côté colonne, selon le schéma retenu.
3. Respectez la même polarité pour les 64 diodes. Le sens de la bande de la diode doit être indiqué explicitement sur le futur schéma et correspondre à la convention `col2row` de la configuration utilisée. Ne vous fiez pas uniquement à une photo ou à une convention générique : confirmez le sens avec le schéma et le firmware du projet.
4. Reliez l’autre côté des diodes aux colonnes selon le plan.
5. Inspectez chaque soudure, retirez les brins ou excès d’étain et vérifiez l’absence de court-circuit entre pistes voisines avant de connecter le contrôleur.

L’auteur a réalisé son prototype avec du **cuivre rigide de 0,8 mm**, mais ne recommande pas cette méthode aux débutants : les croisements et les zones dénudées sont plus difficiles à isoler et peuvent se déplacer. Préférez des fils souples isolés pour un premier montage.

### Plan de matrice

Le mappage touche par touche (ligne, colonne, position physique et nom de touche) sera ajouté lorsque le plan de montage aura été vérifié. En attendant, ne considérez pas les numéros ci-dessous comme un brochage réel :

| Ligne | Broches contrôleur | Colonnes utilisées | Positions non utilisées |
|---|---|---|---|
| L0 à L4 | À relever dans la configuration ZMK | C0 à C13, à cartographier | À cartographier |

**Brochage, sens des diodes et table complète des touches : à compléter après validation sur le clavier assemblé.**

## Contrôleur et batterie

Fixez le contrôleur **nRF52840** de façon à protéger ses soudures et à éviter qu’il touche le câblage dénudé. Isolez mécaniquement toute zone conductrice proche du boîtier.

Le câblage prévu pour cette réalisation est une batterie LiPo **250 mAh** avec le **B+ passant par l’interrupteur** et le **B− relié directement** au retour prévu sur le contrôleur. Identifiez d’abord les bornes exactes du modèle de carte utilisé : ne supposez pas que les broches portant des noms différents sont interchangeables.

- Interrupteur ouvert, vérifiez au multimètre qu’il coupe bien le positif de la batterie.
- Vérifiez soigneusement la polarité avant de brancher la LiPo ; une inversion peut endommager la carte ou la batterie.
- N’alimentez pas simultanément le montage par USB et par la batterie sans avoir confirmé que la carte gère cette situation.
- Ne soudez pas directement sur la cellule LiPo. Isolez les connexions et empêchez tout pincement, perforation ou contact avec des parties chaudes.
- Utilisez uniquement le circuit de charge et les conditions de charge adaptés à la carte et à la batterie.

Le schéma de ce raccordement et la référence exacte de la carte seront ajoutés après vérification des composants utilisés.

## Firmware ZMK

Le firmware public associé au clavier est documenté ici :

**[Freezyi420/wireless-60-handwired-keyboard](https://github.com/Freezyi420/wireless-60-handwired-keyboard)**

Suivez son README pour récupérer ou compiler les fichiers adaptés à cette réalisation, puis flasher le nRF52840 selon les instructions de la carte. Vérifiez que la disposition des broches, les 5 lignes, les 14 colonnes, les six positions absentes et la direction **`col2row`** correspondent au câblage réel avant de flasher.

Les procédures de démarrage en bootloader, de réinitialisation des réglages Bluetooth et de compilation peuvent varier selon la carte et la version du firmware : référez-vous aux instructions à jour du dépôt firmware.

## Tests avant fermeture

1. Sans batterie, contrôlez visuellement les soudures, la polarité des diodes et l’absence de ponts d’étain.
2. À l’aide d’un multimètre, vérifiez la continuité des lignes et colonnes, puis recherchez les courts-circuits entre elles et entre alimentation et masse.
3. Connectez le contrôleur sans batterie lorsque c’est possible et vérifiez qu’il est reconnu en USB.
4. Flashez le firmware correspondant à la matrice réelle.
5. Testez chaque touche une par une. Notez les touches absentes, doublées ou placées sur une mauvaise ligne/colonne.
6. Corrigez le câblage ou la configuration, puis recommencez le test complet.
7. Testez Bluetooth et l’interrupteur avant de ranger les fils et de fermer le boîtier.
8. Fermez le boîtier sans pincer de fil et refaites un dernier test de toutes les touches.

## Dépannage

| Symptôme | Vérifications à faire |
|---|---|
| Une touche ne répond pas | Switch, diode, soudure, continuité de la ligne et de la colonne, puis mappage de la touche dans ZMK |
| Plusieurs touches d’une ligne ou colonne sont absentes | Fil commun, soudure du bus, broche configurée et continuité |
| Des touches apparaissent inversées ou en double | Table de matrice et association ligne/colonne dans la configuration |
| Plusieurs touches s’activent ensemble | Court-circuit, brin de cuivre, diode contournée ou isolation insuffisante |
| Le contrôleur ne démarre pas | Câble USB de données, procédure de bootloader propre à la carte, firmware et connexions d’alimentation |
| Le clavier ne s’appaire plus | Suivre la procédure de réinitialisation des paramètres Bluetooth décrite dans le dépôt firmware |
| L’autonomie est anormalement faible | Interrupteur, consommation au repos, paramètres ZMK et batterie réellement utilisée |

Avant chaque correction électrique, débranchez l’USB et la batterie. Ne laissez pas une panne de matrice sous tension sans surveillance.

## Photos, schémas et fichiers à venir

Ces éléments sont prévus, mais **ne sont pas encore présents dans le dépôt** :

- **Photos du montage :** impression, plateau équipé, câblage avant fermeture et clavier terminé.
- **Schéma Draw.io :** matrice annotée, repères des lignes et colonnes, polarité des diodes et raccordement du contrôleur et de la batterie.
- **Table de matrice :** correspondance vérifiée entre chaque touche physique, ligne, colonne et entrée ZMK.
- **Fichiers 3MF :** pièces adaptées à cette réalisation, sous réserve des droits de redistribution du modèle d’origine.

## Crédits et ressources

- **Modèle original :** [60 Keyboard with Arrow Keys — MakerWorld](https://makerworld.com/fr/models/557186-60-keyboard-with-arrow-keys). Modèle créé par son auteur ; consulter sa page pour la licence, les fichiers et les instructions. Ce dépôt ne republie pas ses fichiers.
- **Guide de câblage manuel :** [keyboard-manual — kanefron5](https://github.com/kanefron5/keyboard-manual).
- **Firmware ZMK public de cette réalisation :** [wireless-60-handwired-keyboard — Freezyi420](https://github.com/Freezyi420/wireless-60-handwired-keyboard).
- **Comparatif de cartes nRF52840 :** [nRFMicro Wiki — Alternatives](https://github.com/joric/nrfmicro/wiki/Alternatives).

Ce guide décrit les étapes et notes originales de fabrication de l’auteur. Les fichiers de conception originaux ne sont pas inclus ; toute future publication de fichiers ou de contenu adapté respectera la licence du modèle source. Aucune licence distincte n’est ajoutée tant que les créations originales à licencier ne sont pas définies.
