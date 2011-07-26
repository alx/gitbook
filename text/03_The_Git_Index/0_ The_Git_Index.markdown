## L'Index Git ##

L'index Git est une zone d'assemblage entre votre répertoire de travail
et votre dépôt. Vous pouvez utiliser l'index pour construire un groupe de
changements qui seront committés ensemble. Quand vous créez un commit, ce
qui se trouve dans l'index est ce qui est committé, et non ce qui se trouve
dans le répertoire de travail.

### A l'Intérieur de l'Index ###

La façon la plus simple de voir ce qu'est l'index est d'utiliser la commande
linkgit:git-status[1]. Quand vous lancez git status, vous pouvez voir quels
fichiers sont assemblés (actuellement dans l'index), quels sont ceux
modifiés mais pas assemblés, et ceux qui ne sont pas suivis.

    $>git status
    # On branch master
    # Your branch is behind 'origin/master' by 11 commits, and can be fast-forwarded.
    #
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #	modified:   daemon.c
    #
    # Changed but not updated:
    #   (use "git add <file>..." to update what will be committed)
    #
    #	modified:   grep.c
    #	modified:   grep.h
    #
    # Untracked files:
    #   (use "git add <file>..." to include in what will be committed)
    #
    #	blametree
    #	blametree-init
    #	git-gui/git-citool

Si vous effacez complètement l'index, vous ne perdrez généralement aucune
information tant que vous avez le nom du tree qui est décrit.

Grâce à cela, vous devriez avoir une bonne compréhension des bases de ce que
Git fait en arrière plan, et pourquoi il est différent de la plupart des
autres systèmes de contrôle de version. Ne vous inquiétez pas si vous
n'avez pas encore tout compris; nous reviendrons sur tous ces point dans
les prochaines parties du livre. Maintenant, nous sommes prêts à installer,
configurer et utiliser Git.
