# Fichier Distribution

L'index de distribution (le fichier `distribution.json`) est écrit en JSON, en voici un exemple :

```json
{
    "version": "1.0.0",
    "discord": {
        "clientId": "12345678901234567890",
        "smallImageText": "RandomServ",
        "smallImageKey": "seal-circle"
    },
    "rss": "https://randomserv.com/articles/index.rss",
    "servers": [
        {
            "id": "Example_Server",
            "name": "Serveur Fictif",
            "description": "Un petit serveur pour l'exemple du distribution.json!",
            "icon": "http://randomserv.com/files/example_icon.png",
            "version": "0.0.1",
            "address": "mc.randomserv.com",
            "minecraftVersion": "1.11.2",
            "discord": {
                "shortId": "Example",
                "largeImageText": "Un serveur 100% fictif",
                "largeImageKey": "server-example"
            },
            "mainServer": true,
            "autoconnect": true,
            "modules": [
                "Insérer les modules ici"
            ]
        }
    ]
}
```

## Objets de l'index

#### Exemple
```JSON
{
    "version": "1.0.0",
    "discord": {
        "clientId": "12334567890123456789",
        "smallImageText": "RandomServ",
        "smallImageKey": "seal-circle"
    },
    "rss": "https://randomserv.com/articles/index.rss",
    "servers": []
}
```

### `version`

La version du fichier `distribution.json`, sera utilisée dans un futur plus ou moins proche por gérer les mises à jour.
Doit être changé à **chaque** modification du fichier.

### `discord`

Paramètres pour [Discord Rich Presence](https://discordapp.com/developers/docs/rich-presence/how-to).

**Propriétés**

* `discord.clientId` - Client ID de l'application Discord.
* `discord.smallImageText` - Texte affiché quand on passe la souris sur la petite image.
* `discord.smallImageKey` - Nom de l'image uploadée sur Discord utilisée pour la petite image.


### `rss`

URL de flux RSS.

---

## Objet serveur

#### Exemple
```JSON
{
    "id": "Example_Server",
    "name": "Serveur fictif",
    "description": "Un serveur 100% fictif!",
    "icon": "http://randomserv.com/files/example_icon.png",
    "version": "0.0.1",
    "address": "mc.randomserv.com",
    "minecraftVersion": "1.11.2",
    "discord": {
        "shortId": "Example",
        "largeImageText": "Example Server",
        "largeImageKey": "server-example"
    },
    "mainServer": true,
    "autoconnect": true,
    "modules": []
}
```

### `id`

L'ID du serveur. Le launcher sauvegarde les modifications en utilisant l'ID. Si l'ID change, toutes les configurations de l'ancien ID **seront éffacées**.

### `name`

Le nom du serveur affiché dans le launcher

### `description`

Description breve, affichée sur le launcher

### `icon`

URL vers l'icone du serveur, affichée sur le launcher

### `version`

La version de la config du serveur

### `address`

L'adresse IP du serveur

### `minecraftVersion`

La version de Minecraft qu'utilise le serveur

### `discord`

Paramètres pour [Discord Rich Presence](https://discordapp.com/developers/docs/rich-presence/how-to).

**Propriété**

* `discord.shortId: string` - Identifiant pour le serveur, utilisé pour afficher `Serveur: shortId` sur Discord.
* `discord.largeImageText: string` - Texte affiché au survol de l'image large.
* `discord.largeImageKey: string` - Nom de l'imag uploadée sur Discord pour l'image large.

### `mainServer`

Un seul serveur du tableau doit avoir la propriété `mainServer` activée. Cela indiquera au lanceur qu'il s'agit du serveur par défaut pour sélectionner si le serveur précédemment sélectionné est invalide ou s'il n'y a pas de serveur précédemment sélectionné. Si ce champ n'est défini par aucun serveur (évitez cela), le premier serveur sera sélectionné par défaut. Si plusieurs serveurs ont activé `mainServer`, le premier que le lanceur trouve sera la valeur effective. Les serveurs qui ne sont pas la valeur par défaut peuvent omettre cette propriété plutôt que de la définir explicitement sur false.

### `autoconnect`

Indique si le serveur peut être connecté automatiquement ou non. Si la valeur est false, le serveur ne sera pas connecté automatiquement même lorsque l'utilisateur a activé le paramètre de connexion automatique.

### `modules: Module[]`

Liste des modules.

---

## Objet Modules

Un module est une représentation générique d'un fichier requis pour exécuter le client minecraft.

#### Example
```JSON
{
    "id": "com.example:artifact:1.0.0@jar.pack.xz",
    "name": "Artifact 1.0.0",
    "type": "Library",
    "artifact": {
        "size": 4231234,
        "MD5": "7f30eefe5c51e1ae0939dab2051db75f",
        "url": "http://files.site.com/maven/com/example/artifact/1.0.0/artifact-1.0.0.jar.pack.xz"
    },
    "subModules": [
        {
            "id": "examplefile",
            "name": "Example File",
            "type": "File",
            "artifact": {
                "size": 23423,
                "MD5": "169a5e6cf30c2cc8649755cdc5d7bad7",
                "path": "examplefile.txt",
                "url": "http://files.site.com/examplefile.txt"
            }
        }
    ]
}
```

Le module parent sera stocké dans le style maven, son chemin de destination sera résolu par son id. Le sous-module a un «chemin» déclaré, donc cette valeur sera utilisée.

### `id`

L'ID du module. Tous les modules qui ne sont pas de type «File» **DOIVENT** utiliser un identifiant maven. Les informations de version et autres métadonnées sont extraites de l'identifiant. Les modules stockés dans le style maven utilisent l'identifiant pour résoudre le chemin de destination. Si l '«extension» n'est pas fournie, la valeur par défaut est «jar».

**Template**

`my.group:arifact:version@extension`

`my/group/artifact/version/artifact-version.extension`

**Exemple**

`net.minecraft:launchwrapper:1.12` OU `net.minecraft:launchwrapper:1.12@jar`

`net/minecraft/launchwrapper/1.12/launchwrapper-1.12.jar`

Si l'artefact du module ne déclare pas la propriété `path`, son chemin sera résolu à partir de l'ID.

### `name`

Le nom du module. Utilisé sur l'interface utilisateur.

### `type`

Le type du module.

### `required`

**OPTIONNEL**

Définit si le module est requis ou non. S'il est omis, le module sera requis.

Seulement pour les modules du type :
* `ForgeMod`
* `LiteMod`
* `LiteLoader`


### `artifact`

L'artefact de téléchargement pour le module.

### `subModules`

**OPTIONNEL**

Un tableau de sous-modules déclarés par ce module. En règle générale, les fichiers qui nécessitent d'autres fichiers sont déclarés en tant que sous-modules. Un exemple rapide serait un mod et le fichier de configuration de ce mod. Les sous-modules peuvent également déclarer leurs propres sous-modules. Le fichier est analysé de manière récursive, il n'y a donc pas de limite.

## Artefacts

Le format de l'artefact du module dépend de plusieurs choses. Le facteur le plus important est l'endroit où le fichier sera stocké. Si vous fournissez un simple fichier à placer dans le répertoire racine des fichiers client, vous pouvez décider de formater le module comme le module `exemplefile` déclaré ci-dessus. Ce module fournit une option `path`, vous permettant de définir directement où le fichier sera enregistré. Seul le «chemin» affectera le fichier final téléchargé.

D'autres fois, vous voudrez peut-être stocker les fichiers de style maven, comme avec des bibliothèques et des mods. Dans ce cas, vous devez déclarer le module comme exemple d'artefact ci-dessus. Le module `id` sera utilisé pour résoudre le chemin final, remplaçant effectivement la propriété` path`. Il doit être fourni au format maven. Plus d'informations à ce sujet sont fournies dans la documentation de la propriété `id`.

Les chemins résolus / fournis sont ajoutés à un chemin de base en fonction du type déclaré du module.

| Type | Chemin |
| ---- | ---- |
| `ForgeHosted` | ({`commonDirectory`}/libraries/{`path` OR resolved}) |
| `LiteLoader` | ({`commonDirectory`}/libraries/{`path` OR resolved}) |
| `Library` | ({`commonDirectory`}/libraries/{`path` OR resolved}) |
| `ForgeMod` | ({`commonDirectory`}/modstore/{`path` OR resolved}) |
| `LiteMod` | ({`commonDirectory`}/modstore/{`path` OR resolved}) |
| `File` | ({`instanceDirectory`}/{`Server.id`}/{`path` OR resolved}) |

Les valeurs `commonDirectory` et` instanceDirectory` sont stockées dans le fichier config.json du lanceur.

### `size`

Taille de l'artefact

### `MD5`

Le hash MD5 de l'artefact

### `path`

**OPTIONNEL**

Un chemin relatif vers l'emplacement où le fichier sera enregistré. Ceci est ajouté au chemin de base du type déclaré du module.

Si cela n'est pas spécifié, le chemin sera résolu en fonction de l'ID du module.

### `url`

L'URL de téléchargement

## Objet requis

### `value`

**OPTIONNEL**

Si le module est requis. La valeur par défaut est true si cette propriété est omise.

### `def`

**OPTIONAL**

Si le module est activé par défaut. N'a aucun effet à moins que «Required.value» soit false. La valeur par défaut est true si cette propriété est omise.

---

## Module Types

### ForgeHosted

Le type de module «ForgeHosted» représente la forge elle-même. Actuellement, le lanceur ne prend en charge que les serveurs de forge, car les serveurs vanilla peuvent être connectés via le lanceur mojang. La partie `Hosted` est la clé, cela signifie que le module de forge doit déclarer ses bibliothèques requises comme sous-modules.
Ex.

```json
{
    "id": "net.minecraftforge:forge:1.11.2-13.20.1.2429",
    "name": "Minecraft Forge 1.11.2-13.20.1.2429",
    "type": "ForgeHosted",
    "artifact": {
        "size": 4450992,
        "MD5": "3fcc9b0104f0261397d3cc897e55a1c5",
        "url": "http://files.minecraftforge.net/maven/net/minecraftforge/forge/1.11.2-13.20.1.2429/forge-1.11.2-13.20.1.2429-universal.jar"
    },
    "subModules": [
        {
            "id": "net.minecraft:launchwrapper:1.12",
            "name": "Mojang (LaunchWrapper)",
            "type": "Library",
            "artifact": {
                "size": 32999,
                "MD5": "934b2d91c7c5be4a49577c9e6b40e8da",
                "url": "http://mc.westeroscraft.com/WesterosCraftLauncher/files/1.11.2/launchwrapper-1.12.jar"
            }
        }
    ]
}
```

Toutes les bibliothèques requises de forge sont déclarées dans le fichier `version.json` qui se trouve à la racine du fichier jar forge. Ces bibliothèques DOIVENT être hébergées et déclarées qu'un sous-module ou une forge ne fonctionnera pas.

Il était prévu d'ajouter un type `Forge`, dans lequel les bibliothèques requises seraient résolues par le lanceur et téléchargées à partir des serveurs de forge. Cependant, les serveurs de forge sont parfois en panne, ce plan a donc été arrêté à moitié mis en œuvre.

---

### LiteLoader

Le type de module «LiteLoader» représente liteloader. Il est traité comme une bibliothèque et ajouté au chemin de classe lors de l'exécution. Des conditions de lancement spéciales sont exécutées lorsque liteloader est présent et activé. Ce module peut être optionnel et basculer de la même manière que les modules «ForgeMod» et «Litemod».
Ex.
```json
{
    "id": "com.mumfrey:liteloader:1.11.2",
    "name": "Liteloader (1.11.2)",
    "type": "LiteLoader",
    "required": {
        "value": false,
        "def": false
    },
    "artifact": {
        "size": 1685422,
        "MD5": "3a98b5ed95810bf164e71c1a53be568d",
        "url": "http://mc.westeroscraft.com/WesterosCraftLauncher/files/1.11.2/liteloader-1.11.2.jar"
    },
    "subModules": [
        "All LiteMods go here"
    ]
}
```

---

### Library

Le type de module «Bibliothèque» représente un fichier de bibliothèque qui sera nécessaire pour démarrer le processus minecraft. Chaque module de bibliothèque sera ajouté dynamiquement à l'argument `-cp` (classpath) lors de la construction du processus de jeu.
Ex.

```json
{
    "id": "net.sf.jopt-simple:jopt-simple:4.6",
    "name": "Jopt-simple 4.6",
    "type": "Library",
    "artifact": {
        "size": 62477,
        "MD5": "13560a58a79b46b82057686543e8d727",
        "url": "http://mc.westeroscraft.com/WesterosCraftLauncher/files/1.11.2/jopt-simple-4.6.jar"
    }
}
```

---

### ForgeMod

Le type de module «ForgeMod» représente un mod chargé par le Forge Mod Loader (FML). Ces fichiers sont stockés dans le style maven et transmis à FML en utilisant le [format Modlist] de forge (https://github.com/MinecraftForge/FML/wiki/New-JSON-Modlist-format).
Ex.
```json
{
    "id": "com.westeroscraft:westerosblocks:3.0.0-beta-6-133",
    "name": "WesterosBlocks (3.0.0-beta-6-133)",
    "type": "ForgeMod",
    "artifact": {
        "size": 16321712,
        "MD5": "5a89e2ab18916c18965fc93a0766cc6e",
        "url": "http://mc.westeroscraft.com/WesterosCraftLauncher/prod-1.11.2/mods/WesterosBlocks.jar"
    }
}
```

---

### LiteMod

Le type de module «LiteMod» représente un mod chargé par liteloader. Ces fichiers sont stockés dans le style maven et transmis à liteloader en utilisant le [format Modlist] de forge (https://github.com/MinecraftForge/FML/wiki/New-JSON-Modlist-format). La documentation pour l'implémentation de liteloader de ceci peut être trouvée sur [ce problème] (http://develop.liteloader.com/liteloader/LiteLoader/issues/34).
Ex.
```json
{
    "id": "com.mumfrey:macrokeybindmod:0.14.4-1.11.2@litemod",
    "name": "Macro/Keybind Mod (0.14.4-1.11.2)",
    "type": "LiteMod",
    "required": {
        "value": false,
        "def": false
    },
    "artifact": {
        "size": 1670811,
        "MD5": "16080785577b391d426c62c8d3138558",
        "url": "http://mc.westeroscraft.com/WesterosCraftLauncher/prod-1.11.2/mods/macrokeybindmod.litemod"
    }
}
```

---

### File

Le type de module «fichier» représente un fichier générique requis par le client, un autre module, etc. Ces fichiers sont stockés dans le répertoire d'instance du serveur.

Ex.

```json
{
    "id": "com.westeroscraft:westeroscraftrp:2017-08-16",
    "name": "WesterosCraft Resource Pack (2017-08-16)",
    "type": "file",
     "artifact": {
        "size": 45241339,
        "MD5": "ec2d9fdb14d5c2eafe5975a240202f1a",
        "path": "resourcepacks/WesterosCraft.zip",
        "url": "http://mc.westeroscraft.com/WesterosCraftLauncher/prod-1.11.2/resourcepacks/WesterosCraft.zip"
    }
}
```
