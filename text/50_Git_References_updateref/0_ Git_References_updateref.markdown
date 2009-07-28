## Les Références Git ##

Les branches, les branches de suivie distantes et les tags sont tous
des références à des commits. Toutes les références sont nommées avec
un nom de chemin séparé par des slash commençant par "refs". Les noms
que nous avons utilisés jusqu'à maintenant sont actuellement des raccourcis:

	- La branche "test" est un raccourci pour "refs/heads/test".
	- Le tag "v2.6.18" est un raccourci pour "refs/tags/v2.6.18".
	- "origin/master" est un raccourci pour "refs/remotes/origin/master".

Le nom complet est parfois utile si, par exemple, il existe un tag et
une branche avec le même nom.

(les références fraîchement créées sont stockées dans le dossier .git/refs,
sous le chemin donné par leur nom. Cependant, pour plus d'efficacité, elles
peuvent aussi être packagées ensemble dans un seul fichier; 
voir linkgit:git-pack-refs[1]).

Un autre raccourci utile, le "HEAD" d'un dépôt peut être référencé en utilisant
simplement le nom de ce dépôt. Par exemple, "origin" est généralement utilisé
comme raccourci vers la branche HEAD dans le dot "origin".

Pour obtenir la liste complète des chemins que git utilise comme référence,
et l'ordre utilisé pour décider laquelle choisir quand il y a plusieurs
références avec le même raccourci, voir la section "SPECIFYING
REVISIONS" de linkgit:git-rev-parse[1].

### Montrer les commits présents uniquement dans une branche ###

Supposons que vous voulez voir tous les commits atteignable depuis la
branche centrale nommée "master" mais non-visible depuis les autres branches
de votre dépôt.

Nous pouvons faire la liste des sommets du dépôts avec linkgit:git-show-ref[1] :

    $ git show-ref --heads
    bf62196b5e363d73353a9dcf094c59595f3153b7 refs/heads/core-tutorial
    db768d5504c1bb46f63ee9d6e1772bd047e05bf9 refs/heads/maint
    a07157ac624b2524a059a3414e99f6f44bebc1e7 refs/heads/master
    24dbc180ea14dc1aebe09f14c8ecf32010690627 refs/heads/tutorial-2
    1e87486ae06626c2f31eaa63d26fc0fd646c8af2 refs/heads/tutorial-fixes

Nous pouvons afficher seulement le nom de la branche, et enlever "master"
avec l'aide des outils standards comme cut et grep :

    $ git show-ref --heads | cut -d' ' -f2 | grep -v '^refs/heads/master'
    refs/heads/core-tutorial
    refs/heads/maint
    refs/heads/tutorial-2
    refs/heads/tutorial-fixes

Et ensuite nous pouvons demander à voir tous les commits
visibles depuis "master" mais pas des autres sommets :

    $ gitk master --not $( git show-ref --heads | cut -d' ' -f2 |
    				grep -v '^refs/heads/master' )

Évidemment, des variations illimités sont possibles, par exemple, pour
afficher tous les commits visible de certains sommets mais pas de ceux
contenant des tags dans le dépôt :

    $ gitk $( git show-ref --heads ) --not  $( git show-ref --tags )

(Voir linkgit:git-rev-parse[1] pour plus d'explications sur la syntaxe
de sélection de commits comme `--not`.)

(!!update-ref!!)
