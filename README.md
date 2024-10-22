# TP3-UNIX


## Exercice 1 : Paramètres

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
   ```bash
    root@serveur-correction:~/tp03# ./analyse.sh ab cd ef yz
    Bonjour, vous avez rentré 4 paramètres.
    Le nom du script est ./analyse.sh.
    Le 3ème paramètre est ef.
    Voici la liste des paramètres : ab cd ef yz
    ```
## Exercice 2 : Vérification du nombre de paramètres

- Création du fichier concat.sh : cat > concat.sh puis ctrl+c
- Modification de concat.sh : nano concat.sh
   ```bash
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

## Exercice 3 : Argument type et droits

- Script : 
  ```bash
  #!/bin/bash

  # Vérification si il y a un argument
  if [ $# -ne 1 ]; then 
          echo "Erreur : vous devez spécifier le chamin d'un fichier."
          exit 1
  fi
  
  fichier="$1"

  # Vérifie si le fichier existe
  if [ ! -e "$fichier" ]; then
          echo "Le fichier $fichier n'existe pas"
          exit 1
  fi

  # Vérifie si le fichier est un répertoire 
  if [ -d "$fichier" ]; then
          echo "Le fichier $fichier est un repertoire"
  # Vérifie si c'est un fichier ordinaire
  elif [ -f "$fichier" ]; then
          # Vérifie si il est vide
          if [ -s "$fichier" ]; then
                  echo "Le fichier $fichier est un fichier ordinaire non vide"
          else
                  echo "Le fichier $fichier est un fichier ordinaire vide"
          fi
  # Vérifie si c'est un lien 
  elif [ -L "$fichier" ]; then
          echo "Le fichier $fichier est un lien symbolique"
  else
          echo "Le fichier $fichier est d'un autre type"
  fi
  
  user=$(whoami)
  permission=""

  # Vérifie la permission de lecture
  if [ -r "$fichier" ]; then
          permission+="lecture "
  fi
  # Vérifie la permission d'écrire
  if [ -w "$fichier" ]; then
          permission+="écriture "
  fi
  # Vérifie la permission d'exécution
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
- Script : 
  ```bash
  #!/bin/bash

  # Vérification si il y a un argument 
  if [ $# -ne 1 ]; thenLister les utilisateurs us
          echo "Erreur : vous devez spécifier un repertoire"
          exit 1
  fi

  # Vérifie si le répertoire existe
  if [ ! -e "$1" ]; then
          echo "Le répertoire n'existe pas."
          exit 1
  fi

  # Vérifie si l'argument passé est un répertoire
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
- Résultat : 
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
- Script : 
  ```bash
  #!/bin/bash
  
  # -F: : Le délimiteur de champ est ':'
  # $3 > 100 : Filtre le 3e champ lorsque qu'il est supérieur a 100
  # { print $1 } : Affiche le premier champ  
  
  awk -F: '$3 > 100 { print $1 }' /etc/passwd 
  ```
- Résultat : 
  ```bash
  root@serveur-correction:~/tp03# ./userdir.sh
  nobody
  systemd-network
  systemd-timesync
  sshd
  ```


## Exercice 6 : Mon utilisateur existe t’il
- Script :
  ```bash
  #!/bin/bash

  # Verifie si il a y 2 paramètres
  if [ $# -ne 2 ]; then
          echo "Erreur : Il faut 2 paramètres."
          exit 1
  fi

  # Recherche si l'utilisateur existe dans /etc/passwd avec les paramètres
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


## Exercice 7 : Création utilisateur
- Script :
  ```bash
  #!/bin/bash
  
  #Vérification si l'utilisateur est root
  if [ "$USER" != "root" ]; then
      echo "Vous devez être root pour exécuter ce script."
      exit 1
  fi

  # Vérification si l'utilisateur existe déjà
  function check_user_existence {
      local username=$1
      if id "$username" &>/dev/null; then
          echo "L'utilisateur $username existe déjà."
          exit 1
      fi
  }

  # Vérification si un répertoire home existe déjà
  function check_home_directory_existence {
      local home_dir=$1
      if [ -d "$home_dir" ]; then
          echo "Le répertoire $home_dir existe déjà."
          exit 1
      fi
  }

  # Infos de l'utilisateur
  read -p "Entrez le login de l'utilisateur: " login
  read -p "Entrez le nom de famille de l'utilisateur: " nom
  read -p "Entrez le prénom de l'utilisateur: " prenom
  read -p "Entrez l'UID (identifiant utilisateur): " uid
  read -p "Entrez le GID (identifiant du groupe): " gid
  read -p "Entrez un commentaire (ex. Nom complet, rôle...): " commenta>

  # Vérification si l'utilisateur existe déjà
  check_user_existence "$login"

  # Définition du répertoire home de l'utilisateur
  home_dir="/home/$login"

  # Vérification si le répertoire home existe déjà
  check_home_directory_existence "$home_dir"

  # Création de l'utilisateur avec les infos
  useradd -m -d "$home_dir" -u "$uid" -g "$gid" -c "$prenom $nom, $comm>

  # Vérifier si la commande useradd s'est exécutée correctement
  if [ $? -eq 0 ]; then
      echo "L'utilisateur $login a été créé avec succès."
  else
      echo "Échec de la création de l'utilisateur."
      exit 1
  fi

  # Succès
  echo "Le répertoire home $home_dir a été créé."
  ```
  
- Résultat :
  ```bash
  root@serveur1:~/tp03# ./createuser.sh
  Entrez le login de l'utilisateur: moi
  Entrez le nom de famille de l'utilisateur: k
  Entrez le prénom de l'utilisateur: l
  Entrez l'UID (identifiant utilisateur): 243
  Entrez le GID (identifiant du groupe): 0013
  Entrez un commentaire (ex. Nom complet, rôle...): chef
  useradd warning: moi's uid 243 outside of the UID_MIN 1000 and UID_MAX 60000 range.
  L'utilisateur moi a été créé avec succès.
  Le répertoire home /home/moi a été créé.
  ```

## Exercice 8 :  Lecture au clavier
- Pour quitter "more" , il faut appuyer sur la touche q.
- Pour avancer d'une ligne, il faut appuyer sur la touche entrée.
- vfd
- vefd
- Script :
  ```bash
  #!/bin/bash

  # Vérification si un répertoire a été fourni
  if [ -z "$1" ]; then
      echo "Veuillez spécifier un répertoire."
      exit 1
  fi

  # Vérification si le répertoire existe
  if [ ! -d "$1" ]; then
      echo "Le répertoire '$1' n'existe pas."
      exit 1
  fi

  # Boucle sur chaque fichier txt dans le répertoire
  for fichier in "$1"/*.txt; do
      # Vérifier si le fichier existe (au cas où il n'y aurait pas de fichiers .txt)
      if [ ! -e "$fichier" ]; then
          echo "Aucun fichier texte trouvé dans le répertoire '$1'."
          exit 0
      fi

      # Utilise la commande file pour vérifier si c'est un fichier txt
      if file "$fichier" | grep -q "text"; then
          echo -n "Voulez-vous visualiser le fichier '$fichier' ? (o/n) "
          read reponse
          if [[ "$reponse" =~ ^[oO]$ ]]; then
              more "$fichier"
          fi
      fi
  done
  ```

- test.txt :
  ```bash
  

  Bonjour


  Au Rien

  Bonsoir
  ```
  
- Résultat :
  ```bash
  root@serveur1:~# ./tp03/lecture.sh tp03
  Voulez-vous visualiser le fichier 'tp03/test.txt' ? (o/n) o


  Bonjour


  Au Rien

  Bonsoir
  ```


## Exercice 9 : Appreciation

- Installation de bc :
  ```bash
  root@serveur1:~/tp03# apt install bc
  ```
- Script :
  ```bash
  #!/bin/bash
  while true; do
      # Demander à l'utilisateur de saisir une note ou de quitter
      read -p "Entrez une note (ou 'q' pour quitter) : " input

      # Vérification si l'utilisateur veut quitter
      if [[ "$input" == "q" ]]; then
          echo "Au revoir !"
          exit 0
      fi

      # Vérification si l'entrée est un nombre
      if ! [[ "$input" =~ ^[0-9]+(\.[0-9]+)?$ ]]; then
          echo "Veuillez entrer une note valide ou 'q' pour quitter."
          continue
      fi

      # Comparaison des notes et affiche le bon message
      if (( $(echo "$input >= 16" | bc -l) )); then
          echo "Très bien"
      elif (( $(echo "$input >= 14" | bc -l) )); then
          echo "Bien"
      elif (( $(echo "$input >= 12" | bc -l) )); then
          echo "Assez bien"
      elif (( $(echo "$input >= 10" | bc -l) )); then
          echo "Moyen"
      else
          echo "Insuffisant"
      fi
  done
  ```
- Résultat :
  ```bash
  root@serveur1:~/tp03# ./note.sh
  Entrez une note (ou 'q' pour quitter) : 18
  Très bien
  Entrez une note (ou 'q' pour quitter) : 13
  Assez bien
  Entrez une note (ou 'q' pour quitter) : q
  Au revoir !
  ```
  

  



         
