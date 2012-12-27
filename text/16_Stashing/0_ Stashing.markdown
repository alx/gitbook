## Le Cache (Stashing) ##

Quand vous êtes au milieu d'une grosse modification, il se peut que vous
trouviez un énorme bug qui doit être corrigé rapidement. Vous
aimeriez le corriger avant de continuer vos modifications. Vous pouvez
utiliser linkgit:git-stash[1] pour sauvegarder l'état actuel de votre
travail, et une fois le bug éliminé (ou, éventuellement, en revenant sur
la branche après l'avoir corrigé sur une branche différente), retrouver
les changements du travail en cours.

    $ git stash "travail en cours de la fonctionnalité conquerir-le-monde"

Cette commande sauvegardera vos changements dans le `stash`, et
remettra à zéro votre répertoire de travail et l'index afin de 
correspondre avec le sommet de la branche courante. Après vous pouvez
faire vos corrections comme d'habitude.

    ... éditer et tester ...
    $ git commit -a -m "machin: bidulefix"

Après ça, vous pouvez revenir là où vous étiez en train de travailler
avec `git stash apply`:

    $ git stash apply


### La File de Cache (Stash Queue) ###

Vous pouvez aussi utiliser le cache pour empiler vos changements cachés.
Si vous lancez 'git stash list', vous verrez quelles caches vous avez
sauvegardé:

	$>git stash list
	stash@{0}: WIP on book: 51bea1d... fixed images
	stash@{1}: WIP on master: 9705ae6... changed the browse code to the official repo

Ensuite vous pouvez les sélectionner individuellement pour les appliquer avec
'git stash apply stash@{1}'. Vous pouvez nettoyer la liste avec
'git stash clear'.
