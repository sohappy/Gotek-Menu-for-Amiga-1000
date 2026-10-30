# Gotek Menu for Amiga 1000 (v1.2)

Un menu de lancement optimisé pour le **Gotek avec FlashFloppy**, spécifiquement configuré pour l'**Amiga 1000**. Ce projet permet de transformer votre lecteur de disquettes en un gestionnaire de bibliothèque, avec un accès rapide à vos images `.adf` sans avoir à scroller manuellement sur le Gotek.

## Fonctionnalités (v1.2)

- **Indexation Automatique** : Le menu scanne la racine de la clé USB et liste tous les fichiers commençant par le préfixe `DSKA`.
- **Interface Directe** : Utilise le mode *Direct Access* de FlashFloppy pour lire le catalogue de la clé USB et l'afficher à l'écran de l'Amiga.
- **Boot Optimisé** : Configuration pensée pour garantir que l'Amiga 1000 démarre toujours sur le Kickstart (`slot 000`).
- **Sélecteur Rapide** : Navigation fluide dans la liste des images avec lancement immédiat après sélection.

* * *

## Configuration Matérielle (Jumpers)

Pour que le menu fonctionne, configurez vos cavaliers Gotek comme suit :

- **S0** : Positionné (Le Gotek est configuré comme le lecteur interne DF0).
- **JC** : Retiré/Ouvert (Active le mode "Shugart", compatible Amiga).
- **Firmware** : FlashFloppy **v3.0 ou plus récent** est requis pour supporter le mode *Direct Access*.

* * *

## Configuration de la Clé USB

### 1\. Formatage

La clé USB doit être formatée en **FAT32** (ou FAT16/12). **Attention : l'exFAT n'est pas supporté.**

### 2\. Structure des fichiers

Tous les fichiers nécessaires au menu et au boot doivent être placés à la **racine** de la clé :

| Fichier | Rôle | Note |
| --- | --- | --- |
| `DSKA0000_KICKSTART.adf` | Kickstart | Emplacement 000 (Obligatoire pour le boot) |
| `DSKA0001_MENU.adf` | Le Menu | Emplacement 001 |
| `DSKA0002_...` à `DSKA0999_...` | Vos disquettes | Doivent toujours avoir 4 chiffres |

*Note : Les dossiers sur la clé ne gênent pas, mais le menu ne peut lire que les fichiers situés à la racine.*

* * *

## Configuration du `FF.CFG`

Le fichier `FF.CFG` est crucial. Assurez-vous que les lignes suivantes sont présentes et correctes :

```
interface = jc
max-cyl = 255
nav-mode = indexed
indexed-prefix = "DSKA"
image-on-startup = init
write-protect = no
```

**⚠️ Pourquoi `max-cyl = 255` ?**  
C'est le réglage le plus important. Il permet au menu d'atteindre la piste 255 du Gotek, zone réservée à la communication "Direct Access" pour lire la liste des fichiers de la clé.

* * *

## Utilisation au quotidien

1.  **Allumage** : Le Gotek affiche `000` et l'Amiga charge le Kickstart.
2.  **Accès au Menu** : Appuyez une fois sur le bouton **\[+\]** du Gotek. L'afficheur indique `001`. Le menu se lance automatiquement après 2 secondes.
3.  **Navigation** :
    - Utilisez les flèches (ou joystick) pour parcourir vos disquettes.
    - Appuyez sur **RETURN** (ou bouton FEU) pour lancer l'image sélectionnée.
4.  **Lancement** : L'Amiga redémarre et charge la disquette choisie. Le Gotek affiche alors le numéro de l'emplacement.
5.  **Retour au Menu** : Appuyez sur les deux boutons du Gotek simultanément pour revenir à `000`, puis appuyez sur **\[+\]** pour revenir au menu (`001`).

* * *

## Compilation

Le projet est développé en C et Assembleur 68k.

**Prérequis :**

- Cross-compiler `m68k-linux-gnu-gcc`
- Bibliothèque Python `pyelftools`

**Commande de build :**

```
python3 tools/build.py
```

* * *

##  Licence

Projet open-source pour la communauté Amiga. Libre d'utilisation et de modification.