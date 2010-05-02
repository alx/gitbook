## L'Index Git ##

L'index est un fichier binaire (généralement gardé dans .git/index) qui
contient une liste ordonnées de chemins de fichiers, chacun avec les
permissions et le SHA1 de l'objet blob; linkgit:git-ls-files[1] peut vous
montrer le contenu de l'index :

    $ git ls-files --stage
    100644 63c918c667fa005ff12ad89437f2fdc80926e21c 0	.gitignore
    100644 5529b198e8d14decbe4ad99db3f7fb632de0439d 0	.mailmap
    100644 6ff87c4664981e4397625791c8ea3bbb5f2279a3 0	COPYING
    100644 a37b2152bd26be2c2289e1f57a292534a51a93c7 0	Documentation/.gitignore
    100644 fbefe9a45b00a54b58d94d06eca48b03d40a50e0 0	Documentation/Makefile
    ...
    100644 2511aef8d89ab52be5ec6a5e46236b4b6bcd07ea 0	xdiff/xtypes.h
    100644 2ade97b2574a9f77e7ae4002a4e07a6a38e46d07 0	xdiff/xutils.c
    100644 d5de8292e05e7c36c4b68857c1cf9855e3d2f70a 0	xdiff/xutils.h

Dans une documentation plus ancienne, vous pourrez voir l'index appelé
"cache du répertoire" ou juste "cache". L'index comporte 3 propriétés
importantes :

1. L'index contient toutes les informations nécessaire pour générer
	un unique (déterminé uniquement) objet "tree".
	
	Par exemple, le lancement de linkgit:git-commit[1] génère un objet
	tree depuis l'index, le stocke dans la base de donnée objet, et l'utilise
	comme l'objet tree associé au nouveau commit.

2. L'index permet une comparaison rapide entre les objet tree qu'il définit
	et le tree de travail.

	Il fait ceci en stockant des informations complémentaires pour chaque
	entrée (comme la date de dernière modification). Ces données ne sont pas
	affichées ci-dessus, et ne sont pas stockées dans l'objet tree créé,
	mais elles peuvent être utilisées pour déterminer rapidement quels
	fichiers du répertoire du travail diffèrent de ceux qui sont stockés dans
	l'index. Cela permet à git de gagner du temps sans avoir besoin de lire
	toutes les données pour ce genre de fichiers pour connaître les changements.

3. Il peut représenter efficacement les informations des conflits
	lors des fusions entre différentes versions des objets tree, permettant
	ainsi à chaque chemin de fichier d´être associé avec suffisament
	d'information à propos des "tree" impliqués dans la création d'une fusion
	three-way.

	Nous avons vu dans <<conflict-resolution>> que durant la fusion, l'index
	peut stocker de multiples version d'un même fichier (appelés "stages").
	La troisième colonne dans la sortie de linkgit:git-ls-files[1] ci-dessus
	est le numéro du "stage", et prendra une valeur autre que 0 pour les
	fichiers avec des conflits après fusion.

L'index est donc un sorte de zone d'assemblage temporaire, qui contient
le "tree" sur lequel vous êtes en train de travailler.
