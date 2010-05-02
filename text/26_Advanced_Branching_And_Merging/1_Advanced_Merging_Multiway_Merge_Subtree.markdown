### Merge Multiples ###

Vous pouvez combiner plusieurs branches en même temps en les listant simplement
dans la même commande linkgit:git-merge[1]. Par exemple :

	$ git merge scott/master rick/master tom/master

est l'équivalent de :

	$ git merge scott/master
	$ git merge rick/master
	$ git merge tom/master

### Subtree ###

Il y a des moments où vous voulez inclure le contenu d'un projet développé
indépendamment dans votre projet. Vous pouvez juste récupérer le code de
cet autre projet tant que les chemins ne rentrent pas en conflit.

Les problèmes surviennent quand il y a des fichiers en conflit.
Les candidats potentiels sont les Makefiles et les autres noms de fichiers
standardisés. Vous pouvez fusionner ces fichiers, mais ce n'est pas forcément
ce que vous voulez faire. Une meilleure solution pour ce problème peut être de
fusionner le projet dans un sous-répertoire. Mais la stratégie de fusion ne
supporte la récursivité, donc la simple récupération des fichiers ne
fonctionnera pas.

Vous voudrez alors utiliser la stratégie de fusion subtree, qui vous aidera
dans cette situation.

Dans cet exemple, disons que vous avez un dépôt dans /path/to/B (si vous voulez
ça peut aussi être une adresse URL). Vous voulez fusionner la branche "master" de
ce dépôt dans le sous-répertoire dir-B de la branche actuelle.

Voici la séquence de commande que vous utiliserez :

	$ git remote add -f Bproject /path/to/B (1)
	$ git merge -s ours --no-commit Bproject/master (2)
	$ git read-tree --prefix=dir-B/ -u Bproject/master (3)
	$ git commit -m "Merge B project as our subdirectory" (4)
	$ git pull -s subtree Bproject master (5)
	
Utiliser le fusion subtree vous permet d'apporter moins de complications
administratives aux utilisateurs de votre dépôt. Elle est aussi
compatible avec des versions plus anciennes de git (jusqu'à Git v1.5.2)
et vous aurez le code juste après le clonage.

Cependant, si vous utilisez des sous-modules, vous pouvez alors choisir
de ne pas transférer les objets de ces sous-modules. Cela peut poser
un problème lors de l'utilisation des fusions subtree.

Il est aussi plus facile d'effectuer des changements dans votre autre projet
si vous utiliser les sous-modules.

(from [Using Subtree Merge](http://www.kernel.org/pub/software/scm/git/docs/howto/using-merge-subtree.html))


