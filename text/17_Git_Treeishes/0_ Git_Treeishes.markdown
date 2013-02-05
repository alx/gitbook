## L'Arborescence Git (treeish) ##

Il y a de nombreuses manières de faire référence à un "commit" ou un "tree"
sans épeler toute la chaîne de 40 caractères sha. Avec Git, on appelle ces
références 'treeish'.

### Sha Partiel ###

Si le sha de votre commit est '<code>980e3ccdaac54a0d4de358f3fe5d718027d96aae</code>',
git le reconnaîtra aussi avec les chaînes suivantes:

	980e3ccdaac54a0d4de358f3fe5d718027d96aae
	980e3ccdaac54a0d4
	980e3cc

Tant que le sha partiel est unique - il ne peut pas être confondu avec un autre
(ce qui serait vraiment invraisemblable si vous avez au moins 5 caractères),
git étendra le sha partiel pour vous.

### Branche, Remote ou Nom de Tag ###

Vous pouvez toujours utiliser une branche, un remote ou un nom de tag à la
place d'un sha, puisque qu'ils ne sont que des pointeurs. Si votre branche
"master" est sur le commit 980e3 et que vous la publiez (push) sur 'origin'
et que vous l'avez taggée 'v1.0', alors toutes les références suivantes
sont équivalentes:

	980e3ccdaac54a0d4de358f3fe5d718027d96aae
	origin/master
	refs/remotes/origin/master
	master
	refs/heads/master
	v1.0
	refs/tags/v1.0

Ce qui signifie que les commandes suivantes vous donneront le même résultat:

	$ git log master
	
	$ git log refs/tags/v1.0
	
### Spécification de Date ###

Le Log de références que git conserve vous permettre de faire certaines choses
localement, comme:

	master@{yesterday}

	master@{1 month ago}

Qui est un raccourci pour
'là où se trouver le sommet de la branche master hier', etc. Ce format peut
montrer des sha différents sur des ordinateurs différents, même si la branche
master est actuellement au même niveau.

### Spécification Ordinale ###

Le format suivant vous donnera la Nième valeur précédent une référence
particulière. Par exemple:

	master@{5}

vous donnera la 5ième valeur avant la référence du sommet de master.
	
### La Carotte Parent ###

Cela vous donnera le Nième parent d'un commit particulier. Ce format n'est
utile que pour les commits de merge - les objets commits qui ont plus
d'un parent.

	master^2
	
	
### Spécification avec le Tilde ###

La spécification avec le tilde vous donnera le Nième grand-parent d'un objet
commit. Par exemple,

	master~2

vous donnera le premier parent du premier parent du commit vers lequel pointe
master. C'est l'équivalent de:

	master^^

Vous pouvez aussi continuer à faire ça. Les spécifications suivantes pointent
vers le même commit:

	master^^^^^^
	master~3^~2
	master~6

### Pointeur de Tree ###

Cela facilite la recherche d'un commit en partant de l'arbre vers lequel
il pointe. Si vous avez besoin du sha vers lequel un commit pointe,
vous pouvez ajouter la spécification '^{tree}' à la fin de celui-ci.

	master^{tree}

### Spécification de Blob ###

Si vous voulez le sha d'un blob en particulier, vous pouvez ajouter le chemin
du blob à la fin de l'arborescence, comme ceci:

	master:/chemin/vers/le/fichier
	
### Les Suites ###

Pour terminer, vous pouvez spécifier une suite de commits avec la spécification
de suite. Par exemple, vous pourrez obtenir les commits entre 7b593b5 et 51bea1
(avec 51bea1 le plus récent), en éliminant 7b593b5 mais en gardant 51bea1:

	7b593b5..51bea1

Ceci inclura tous les commits *depuis* 7b593b:

	7b593b.. 
	
	
