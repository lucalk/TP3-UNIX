# TP3-UNIX


## Exercice : paramètres

- Création d'un dossier : mkdir tp03
- Dans tp03, création du fichier analyse.sh : cat > analyse.sh puis ctrl+c
- Modification de analyse.sh : nano analyse.sh
  - ```bash
    #!/bin/bash

    echo "Bonjour, vous avez rentré $# paramètres."

    echo "Le nom du script est $0."
 
    echo "Le 3ème paramètre est $3."

    echo "Voici la liste des paramètres : $@"
    ```
- Résultat :
  - ```bash
    root@serveur-correction:~/tp03# ./analyse.sh ab cd ef yz
    Bonjour, vous avez rentré 4 paramètres.
    Le nom du script est ./analyse.sh.
    Le 3ème paramètre est ef.
    Voici la liste des paramètres : ab cd ef yz
    ```
## Exercice : vérification du nombre de paramètres

- Création du fichier concat.sh : cat > concat.sh puis ctrl+c
- Modification de concat.sh : nano concat.sh
  - ```bash
    #!/bin/bash

    if [ $# -eq 2 ]; then
        echo $1$2 
    else
        echo "Erreur, veuillez entrer SEULEMENT 2 paramètres" 
    fi
    ```
- Résultat :
  - ```bash
    root@serveur-correction:~/tp03# ./concat.sh bonj 222
    bonj222
    ```
  - ```bash
    root@serveur-correction:~/tp03# ./concat.sh bonj 222 ujyh
    Erreur, veuillez entrer SEULEMENT 2 paramètres
    ```

## Exercice : argument type et droits




```bash
#!bin/bash

if [ $# -ne 1 ]; then 
        echo "Erreur : vous debez spécifier le chamin d'un fichier."
        exit 1
fi

fichier="$1"

if [ ! -e "$fichier" ]; then
        echo "Le fichier $fichier n'existe pas"
        exit 1
fi

if [ -d "$fichier" ]; then
        echo "Le fichier $fichier est un repertoire"
elif [ -f "$fichier" ]; then
        if [ -s "$fichier" ]; then
                echo "Le fichier $fichier est un fichier ordinaire non vide"
        else
                echo "Le fichier $fichier est un fichier ordinaire vide"
        fi
elif [ -L "$fichier" ]; then
        echo "Le fichier $fichier est un lien symbolique"
else
        echo "Le fichier $fichier est d'un autre type"
```

```bash
user=$(whoami)
permission=""

if [ -r "$fichier" ]; then
        permission+="lecture"
fi
if [ -w "$fichier" ]; then
        permission+="écriture"
fi
if [ -x "$fichier" ]; then
        permission+="exécution"
fi

if [ -n $"permission" ]; then
        echo "\"$fichier\" est accessible par $user en $permission."
else 
        echo "\"$fichier\" n'est pas accessible par $user."
fi
```



         
