## Usage Basique des Branches et des Merges ##

Un seul dépôt git peut maintenir de nombreuses branches de
développement. Pour créer une nouvelle branche nommée
"experimental", il faut utiliser la commande :

    $ git branch experimental

Lancez maintenant :

    $ git branch

Vous obtiendrez la liste de toutes les branches existantes :

      experimental
    * master

La branche "experimental" est celle que vous venez de créer,
et la branche "master" est la branche par défaut qui a été
créée automatiquement pour vous. L'astérisque signale la
branche su laquelle vous vous trouvez. Tapez :

    $ git checkout experimental

pour passer sur la branche "experimental". Éditez maintenant
un fichier, committez le changement, et revenez sur la
branche "master" :

    (éditer un fichier)
    $ git commit -a
    $ git checkout master

Vous pouvez vérifier que le changement que vous venez de
faire n'est plus visible, puisqu'il a été fait sur la branche
"experimental" et que nous sommes revenus sur la branche
"master".

Vous pouvez faire un changement différent sur la branche
"master" :

    (éditer un fichier)
    $ git commit -a

à partir de maintenant, les 2 branches ont divergé et des changements
différents on été faits dans chacune d'elles. Pour fusionner (merger)
les changements effectués dans la branche "experimental" sur
la branche "master", lancez :

    $ git merge experimental

Si les changements ne créent pas de conflit, vous avez terminé.
S'il y a des conflits, un marquage sera laissé dans les fichiers
problématiques fin de vous montrer le conflit :

    $ git diff

Cette commande vous montrera ces marquages. Une fois que vous avez édité les
fichiers pour résoudre les conflits, tapez :

    $ git commit -a

Afin de committer le résultat du merge. Enfin, lancez la commande :

    $ gitk

Pour admirer la représentation graphique de l'historiqueobtenu.

Maintenant, vous pouvez effacer la branche "experimental" avec

    $ git branch -d experimental

Cette commande s'assure que les changement de la branche
"experimental" se trouve dans la branche courante.

Si vous avez développé une idée saugrenue et que vous la regrettez, vous
pouvez toujours effacer cette branche avec :

    $ git branch -D crazy-idea

Les branches sont faciles à mettre en place et demande peu d'efforts,
c'est donc un bon moyen de tester des choses nouvelles.

### Comment merger ###

Vous pouvez joindre 2 branches de développement divergentes
en utilisant linkgit:git-merge[1] :

    $ git merge titrebranche

merge les changements faits dans la branche "titrebranche" avec la
branche courante. Si il y a des conflits, comme par exemple un 
même fichier modifié au même endroit de 2 façons différentes
dans la branche distante et la branche locale, vous serez avertis.
L'avertissement peut ressembler à quelque chose comme ça :

    $ git merge next
     100% (4/4) done
    Auto-merged file.txt
    CONFLICT (content): Merge conflict in file.txt
    Automatic merge failed; fix conflicts and then commit the result.

Des marqueurs de conflit sont ajoutés aux fichiers problématiques,
et après les avoir résolu manuellement, vous pouvez
mettre à jour l'index avec le nouveau contenu et lancer 
`git commit`, comme vous le feriez quand vous modifiez un
fichier.

Si vous analysez le résultat de ce commit avec gitk, vous verrez
qu'il a 2 parents : l'un pointant vers le sommet de la branche
courante, et l'autre pointant vers le sommet de l'autre branche.

### Résoudre un merge ###

Quand un merge n'est pas résolu automatiquement, git laisse l'index
et le "tree" de travail dans un état spécial vous donnant toutes
les informations dont vous aurez besoin pour vous aider à résoudre
le merge.

Les fichiers en conflits sont marqués spécialement dans l'index,
donc jusqu'à que vous ayez résolu le problème et mis à jour
l'index, linkgit:git-commit[1] ne fonctionnera pas :

    $ git commit
    file.txt: needs merge

De plus, linkgit:git-status[1] vous donnera la liste de ces
fichiers "non-mergés", et les fichiers contenant des conflits
auront les conflits marqués comme ceci :

    <<<<<<< HEAD:file.txt
    Hello world
    =======
    Goodbye
    >>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:file.txt

Vous n'avez plus qu'à éditer ces fichier pour résoudre les conflits,
puis :

    $ git add file.txt
    $ git commit

Notez que le message du commit sera déjà rempli pour vous avec
quelques informations à propos du merge. Normalement vous pouvez
juste laisser ce message inchangé, mais vous pourrez ajouter un 
commentaire additionnel si vous le désirez.

Cette partie contient donc tout ce que vous avez besoin de savoir pour
résoudre un merge simple. Mais git peut vous fournir aussi plus
d'information pour vous aider à résoudre les conflits.

### Annuler un merge ###

Si vous êtes bloqués et que vous décidez de laisser tomber en jetant tous vos
brouillons par la fenêtre, vous pouvez toujours revenir à l'état initial
où vous vous trouviez avant le merge avec la commande :

    $ git reset --hard HEAD

Ou, si vous avez déjà committé le merge que vous voulez mettre à la poubelle,

    $ git reset --hard ORIG_HEAD

Cependant, cette dernière commande peut être dangereuse dans certains
cas--ne jetez jamais un commit si celui-ci est lui même le merge
d'une autre branche, sinon vous risqueriez de rendre confus
les prochains merges.

### Avance rapide des merges ###

Il y a un cas spécial non-mentionné plus tôt, qui est traité différemment.
Normalement, un merge est un commit avec 2 parents, un pour chacune des
2 lignes de développement qui seront mergées.

Cependant, si la branche courante n'a pas divergé de l'autre (tous
les commit présent dans la branche courante sont déjà contenus dans l'autre
branche) alors git ne fait qu'une "avance rapide" : le sommet (head) de la
branche courante est alors avancé jusqu'au point du sommet de la branche à
merger, sans qu'aucun commit ne soit créé.

[gitcast:c6-branch-merge]("GitCast #6: Branching and Merging")
