## Workflow Normal ##

Modifiez quelques fichiers, puis ajoutez leur contenu mis à jour dans
l'index :

    $ git add file1 file2 file3

Vous êtes maintenant prêt pour le commit. Vous pouvez vérifier ce qui
va être committé en utilisant la commande linkgit:git-diff[1] avec
l'option --cached :

    $ git diff --cached

(Sans l'option --cached, linkgit:git-diff[1] vous montrera les
changements que vous avez faits mais que vous n'avez pas encore
ajoutés à l'index.) Vous pouvez aussi trouver un résumé rapide
de la situation en utilisant la commande linkgit:git-status[1] :

    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	modified:   file1
    #	modified:   file2
    #	modified:   file3
    #

Si vous devez faire d'autres changements, faites-le maintenant, puis
ajoutez le contenu modifié à l'index. Enfin, "committez" vos
changements comme ceci :

    $ git commit

Git vous demandera de laisser un message pour décrire les changements
qui ont eu lieu, puis enregistrera une nouvelle version du projet.

Au lieu de lancer `git add` avant vos "commit", vous pouvez aussi
utiliser directement cette commande :

    $ git commit -a

Git trouvera automatiquement les fichiers modifiés (mais pas les nouveaux), 
les ajoutera à l'index et les "committera", en une seule commande.

Note sur le message du "commit" : bien que ce ne soit pas
obligatoire, il est assez efficace de commencer le message du "commit"
avec une courte ligne (moins de 50 caractères) qui résume le
changement, suivi d'une ligne blanche, puis d'une description plus
complète. Les outils qui transforment les commits en mail, par
exemple, utilisent la première ligne du commit pour le sujet du
mail, et le reste pour le contenu.

#### Git surveille le contenu et non les fichiers ####

De nombreux systèmes de contrôle de version fournissent une commande
"add" qui demande au système de surveiller les changements dans
un nouveau fichier. La commande "add" de Git est plus simple et plus puissante :
 `git add` est utilisé à la fois pour les nouveaux fichiers et les fichiers 
nouvellement modifiés. Dans les 2 cas, elle prend une capture des fichiers
fournis et assemble leur contenu dans l'index pour être prêt à être
inclus dans le prochain commit.

[gitcast:c2_normal_workflow]("GitCast #2: Normal Workflow")
