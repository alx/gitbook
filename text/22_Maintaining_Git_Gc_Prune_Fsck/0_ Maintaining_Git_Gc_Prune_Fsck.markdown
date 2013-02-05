## Maintenir Git ##

### Assurer une bonne performance ###

Sur les dépôts de grande taille, git nécessite une compression pour
éviter que les informations de l'historique prennent trop d'espace
sur le disque et en mémoire.

Cette compression n'est pas effectuée automatiquement. Vous devrez donc,
à l'occasion, lancer linkgit:git-gc[1]:

    $ git gc

pour re-compresser l'archive. Cela peut prendre beaucoup de temps,
donc vous préférerez peut-être lancer git-gc quand vous ne travaillez
pas sur autre chose.


### Assurer la fiabilité ###

La commande linkgit:git-fsck[1] lance une série de vérifications de consistance
du dépôt, et rapporte les problèmes. Cela peut aussi prendre un peu de temps.
L'avertissement le plus courant concerne les objets "dangling" (suspendus):

    $ git fsck
    dangling commit 7281251ddd2a61e38657c827739c57015671a6b3
    dangling commit 2706a059f258c6b245f298dc4ff2ccd30ec21a63
    dangling commit 13472b7c4b80851a1bc551779171dcb03655e9b5
    dangling blob 218761f9d90712d37a9c5e36f406f92202db07eb
    dangling commit bf093535a34a4d35731aa2bd90fe6b176302f14f
    dangling commit 8e4bec7f2ddaa268bef999853c25755452100f8e
    dangling tree d50bb86186bf27b681d25af89d3b5b68382e4085
    dangling tree b24c2473f1fd3d91352a624795be026d64c8841f
    ...

Les objets suspendus ne posent aucun problème. Au pire, ils prennent un petit
peu plus d'espace disque. Ils servent parfois de solution de dernier secours
pour récupérer des travaux perdus.
