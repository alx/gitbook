## Comparer les Commits - Git Diff ##

Vous pouvez générer les différences entre deux versionis de votre projet
en utilisant linkgit:git-diff[1] :

    $ git diff master..test

Cette commande produit une différence entre le sommet de 2 banches.
Si vous préférez trouver la différence de leur ancêtre commun,
vous pouvez utiliser 3 points au lieu de 2 :

    $ git diff master...test

linkgit:git-diff[1] est un outils incroyablement utile pour trouver
ce qui a changé entre 2 points dans l'historique de votre projet, ou
pour voir quelle personne a essayé d'introduire une nouvelle
branche, etc.

### Ce que vous committerez ###

Vous utiliserez couramment linkgit:git-diff[1] pour trouver les différences
entre votre dernier commit, votre index, et votre répertoire de travail
courant. Un usage courant est de lancer :
    
    $ git diff

ce qui vous montrera les changements dans le répertoire de travail
qui ne sont pas encore assemblés pour le prochain commit.
Si vous voulez voir ce qui _est_ assemblé pour le prochain commit,
vous pouvez lancer :

    $ git diff --cached

ce qui vous montrera la différence entre l'index et votre dernier commit;
ce que vous committerez si vous lancez "git commit" sans l'option "-a".
Enfin, vous pouvez lancer

    $ git diff HEAD

pour afficher les changements de votre le répertoire de travail depuis
votre dernier commit; ces changements seront committés si vous lancez
"git commit -a".

### Plus d'Options de Diff ###

Si vous voulez voir comment votre répertoire de travail actuel diffère de
l'état  du projet dans une autre branche, vous pouvez lancer quelque chose
comme ça :

    $ git diff test

Cela vous montrera la différence entre votre répertoire de travail actuel
et la capture de la branche 'test'. Vous pouvez aussi limiter la
différence à un fichier spécifique ou à un sous-répertoire en ajoutant
un *limiteur de chemin* :

    $ git diff HEAD -- ./lib 

Cette commande vous montrera les différences entre votre répertoire de
travail actuel et le dernier commit (ou , plus précisément, le sommet de
la branche actuelle), en limitant la comparaison aux fichiers dans le
répertoire 'lib'.

Si vous ne voulez pas voir le patch complet, vous pouvez ajouter
l'option '--stat', qui limitera la sortie aux noms de fichier qui ont
changés, accompagné d'un petit graphe décrivant le nombre de lignes
différentes dans chaque fichier.

    $>git diff --stat
     layout/book_index_template.html                    |    8 ++-
     text/05_Installing_Git/0_Source.markdown           |   14 ++++++
     text/05_Installing_Git/1_Linux.markdown            |   17 +++++++
     text/05_Installing_Git/2_Mac_104.markdown          |   11 +++++
     text/05_Installing_Git/3_Mac_105.markdown          |    8 ++++
     text/05_Installing_Git/4_Windows.markdown          |    7 +++
     .../1_Getting_a_Git_Repo.markdown                  |    7 +++-
     .../0_ Comparing_Commits_Git_Diff.markdown         |   45 +++++++++++++++++++-
     .../0_ Hosting_Git_gitweb_repoorcz_github.markdown |    4 +-
     9 files changed, 115 insertions(+), 6 deletions(-)

Cela permet parfois de voir plus facilement les changements effectués
