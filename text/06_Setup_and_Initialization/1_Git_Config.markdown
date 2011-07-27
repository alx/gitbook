### Configuration de Git ###

La première chose que vous voudrez et devrez faire, c'est de configurer
votre nom et votre adresse mail que Git utilisera quand vous signerez
vos commits.

    $ git config --global user.name "Scott Chacon"
    $ git config --global user.email "schacon@gmail.com"

Ces commandes vont créer un fichier dans votre répertoire utilisateur qui 
sera utilisé par tous vos projets. Par défaut, ce fichier est
*~/.gitconfig* et il contiendra quelque chose comme ça :

    [user]
            name = Scott Chacon
            email = schacon@gmail.com
            
Si vous voulez changer ces valeurs pour un projet spécifique (pour utiliser
un mail d'entreprise par exemple), vous pouvez lancer la commande
*git config*, sans l'option *--global*, depuis l'intérieur de votre projet. 
Cela ajoutera une section [user], comme celle que nous venons
de voir, dans ce fichier *.git/config* placé à la racine de votre projet.
