# TP3-UNIX


## Exercice 1 : paramètres

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
## Exercice 2 : vérification du nombre de paramètres

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

## Exercice 3 : argument type et droits

```bash
#!/bin/bash

if [ $# -ne 1 ]; then 
        echo "Erreur : vous devez spécifier le chamin d'un fichier."
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
fi

user=$(whoami)
permission=""

if [ -r "$fichier" ]; then
        permission+="lecture "
fi
if [ -w "$fichier" ]; then
        permission+="écriture "
fi
if [ -x "$fichier" ]; then
        permission+="exécution "
fi

if [ -n $"permission" ]; then
        echo "\"$fichier\" est accessible par $user en $permission."
else 
        echo "\"$fichier\" n'est pas accessible par $user."
fi
```

Résultat : 
```bash
root@serveur-correction:~/tp03# ./test-fichier.sh analyse.sh 
Le fichier analyse.sh est un fichier ordinaire non vide
"analyse.sh" est accessible par root en lecture écriture exécution .
```
```bash
root@serveur-correction:~# ./tp03/test-fichier.sh ./tp03/concat.sh
Le fichier ./tp03/concat.sh est un fichier ordinaire non vide
"./tp03/concat.sh" est accessible par root en lecture écriture exécution .
```


## Exercice 4 : Afficher le contenu d’un répertoire

```bash
#!/bin/bash

if [ $# -ne 1 ]; thenLister les utilisateurs us
        echo "Erreur : vous devez spécifier un repertoire"
        exit 1
fi

if [ ! -e "$1" ]; then
        echo "Le répertoire n'existe pas."
        exit 1
fi

if [ ! -d "$1" ]; then
        echo "Erreur : $repertoire n'est pas un répertoire."
        exit 1
fi

# affiche chaque dossier du répertoire sur une ligne
echo "#### Les dossiers" 
ls -d "$1"/*/ | xargs -n 1  

# affiche chaque fichier du répertoire sur une ligne
echo "#### Les fichiers"
find "$1" -type f
```
Résultat : 
```bash
root@serveur-correction:~/tp03# ./listedir.sh testQuatre
#### Les dossiers
testQuatre/iuheg/
testQuatre/ofrne/
#### Les fichiers
testQuatre/fich.sh
testQuatre/hir.sh
```


## Exercice 5 : Lister les utilisateurs
```bash
#!/bin/bash

# -F: : Le délimiteur de champ est ':'
# $3 > 100 : Filtre le 3e champ lorsque qu'il est supérieur a 100
# { print $1 } : Affiche le premier champ  

awk -F: '$3 > 100 { print $1 }' /etc/passwd 
```

Résultat : 
```bash
root@serveur-correction:~/tp03# ./userdir.sh
nobody
systemd-network
systemd-timesync
sshd
```

## Exercice 6 : Mon utilisateur existe t’il
```bash
#!/bin/bash

if [ $# -ne 2 ]; then
        echo "Erreur : Il faut 2 paramètres."
        exit 1
fi

user=$(awk -F: -v login="$1" -v uid="$2" '$1==login && $3==uid {print $1}' /etc/passwd)

if [ -n "$user" ]; then
        echo "$2"
else
        echo "rien"
fi

```

Résultat : 

```bash
root@serveur-correction:~/tp03# ./userexist.sh games 5

root@serveur-correction:~/tp03# ./userexist.sh sshd 1010
rien
root@serveur-correction:~/tp03# ./userexist.sh games 5
5
root@serveur-correction:~/tp03# ./userexist.sh sshd 101
101
```



         
