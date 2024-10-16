# TP3-UNIX


## Exercice : paramètres

- Création d'un dossier : mkdir tp03
- Dans tp03, création du fichier analyse.sh : cat > analyse.sh puis ctrl+c
- Modification de analyse.sh :
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

- 
