## Branches et Merges Avancés ##

### Trouver de l'aide pour résoudre les conflits durant un merge ###

Tous les changements que git peut fusionner automatiquement sont déjà ajoutés
à l'index, donc linkgit:git-diff[1] ne vous montre que les conflits.
Il a une syntaxe peu commune:

    $ git diff
    diff --cc fichier.txt
    index 802992c,2b60207..0000000
    --- a/fichier.txt
    +++ b/fichier.txt
    @@@ -1,1 -1,1 +1,5 @@@
    ++<<<<<<< HEAD:fichier.txt
     +Hello world
    ++=======
    + Goodbye
    ++>>>>>>> 77976da35a11db4580b80ae27e8d65caf5208086:fichier.txt

Souvenez-vous que le commit qui sera effectué après que nous ayons
résolu ce conflit aura 2 parents: l'un sera le sommet de la branche courante
(HEAD) et l'autre sera le sommet de la branche qui s'occupe de la fusion,
stockée temporairement dans MERGE_HEAD.

Durant le merge, l'index garde 3 versions de chaque fichier. Une de ces 3
"étapes de fichier" représente une version différente du fichier:

	$ git show :1:fichier.txt	# Le fichier dans l'ancêtre commun des 2 branches
	$ git show :2:fichier.txt	# La version présente dans HEAD.
	$ git show :3:fichier.txt	# La version présente dans MERGE_HEAD.

Quand vous demandez à linkgit:git-diff[1] de vous montrer les conflits, il fait
une différence en 3 points entre les résultats conflictuels de merge dans le
répertoire de travail avec les versions 2 et 3 pour montrer seulement
les morceaux de code qui ont du contenu de chaque côté, mélangés (en d'autres
termes, quand un morceau du résultat de la fusion ne vient que de la version 2,
alors ce morceau n'est pas en conflit est n'est pas affiché. Idem pour la
version 3).

La différence en début de chapitre vous montre les différences entre la version
de travail de fichier.txt et les versions 2 et 3. Donc au lieux de rajouter les
préfixes "+" ou "-" devant chaque ligne, on utilise maintenant 2 colonnes pour
ces préfixes. La première colonne est utilisée pour les différences entre le
premier parent et la copie du répertoire de travail et la deuxième pour les 
différences entre le second parent et la copie du répertoire de travail.
(Voir la section "COMBINED DIFF FORMAT" dans la documentation de
linkgit:git-diff-files[1] pour plus de détails sur ce format.)

Après avoir résolu le conflit de manière évidente (mais avant de mettre à jour
l'index), le diff ressemblera à:

    $ git diff
    diff --cc fichier.txt
    index 802992c,2b60207..0000000
    --- a/fichier.txt
    +++ b/fichier.txt
    @@@ -1,1 -1,1 +1,1 @@@
    - Hello world
    -Goodbye
    ++Goodbye world

Cela montre que notre version résolue a effacé "Hello world" du premier
parent, effacé "Goodbye" du second parent, puis ajouté "Goodbye world",
qui était avant absent des 2.

Quelques options spéciales de diff permettent de faire la différence entre
le répertoire de travail et les différentes étapes:

    $ git diff -1 fichier.txt			# différence avec l'étape 1
    $ git diff --base fichier.txt		# même chose que ci-dessus
    $ git diff -2 fichier.txt			# différence avec l'étape 2
    $ git diff --ours fichier.txt		# même chose que ci-dessus
    $ git diff -3 fichier.txt			# différence avec l'étape 3
    $ git diff --theirs fichier.txt	    # même chose que ci-dessus

Les commandes linkgit:git-log[1] and linkgit:gitk[1] fournissent aussi de
l'aide particulière pour les fusions :

    $ git log --merge
    $ gitk --merge

Cela vous montrera tous les commits qui existent seulement dans HEAD ou dans
MERGE_HEAD et qui concernent les fichiers non fusionnés.

Vous pouvez aussi utiliser linkgit:git-mergetool[1], qui vous permet de
fusionner des fichiers non fusionnés en utilisant des outils externes comme
emacs ou kdiff3.

Chaque fois que vous résolvez les conflits d'un fichier et que vous mettez
à jour l'index :

    $ git add fichier.txt

les différentes étapes de ces fichiers "s'effondreront", après quoi
git-diff ne montrera plus (par défaut) de différence pour ce fichier.
