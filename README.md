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


         
