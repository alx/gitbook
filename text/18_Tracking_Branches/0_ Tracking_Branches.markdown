## Les Branches de Suivi ##

Une 'branche de suivi' de Git est une branche locale qui est connectée à une
branche distante. Quand vous publiez ou récupérez les données de cette branche,
Git publie et récupère automatiquement les informations de la branche à
laquelle elle est connectée.

Utilisez ceci si vous récupérer toujours vos données depuis la même branche
en amont dans une nouvelle branche, et si vous ne voulez pas utiliser 
"git pull <repository> <refspec>" explicitement.

La commande 'git clone' configure automatiquement une branche "master"
qui est une branche de suivi pour 'origin/master' - la branche "master"
du dépôt cloné.

Vous pouvez créer une branche de suivi manuellement en ajoutant l'option
'--track' à la commande 'branch' de Git.

	git branch --track experimental origin/experimental

Puis vous lancez:

	$ git pull experimental

Cela récupérera automatiquement les données de 'origin' et mergera
'origin/experimental' dans votre branche local 'experimental'.

De la même manière, vous pouvez publier vers 'origin', Git publiera vos
modifications de 'experimental' vers 'origin/experimental', sans n'avoir aucune
commande à spécifier.
