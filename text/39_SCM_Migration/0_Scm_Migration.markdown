## Migration depuis un autre SCM ##

Vous avez décidé de migrer depuis un autre système de contrôle de version
et convertir l'intégralité de votre projet vers Git. Comment le faire
facilement?

### Importation depuis Subversion ###

Git est fournit avec un script nommé git-svn : c'est une commande de clonage
qui importe un dépôt subversion dans un nouveau dépôt git. Il existe aussi
un outil gratuit sur le service GitHub qui fera cette manipulation pour
vous.
	
	$ git-svn clone http://my-project.googlecode.com/svn/trunk new-project

Ceci vous donnera un nouveau dépôt Git avec tout l'historique du dépôt
Subversion original. Cela peut prendre beaucoup de temps, puisque qu'il démarre
avec la version 1 du projet subversion puis vérifie et commit localement
chaque révision une par une.

### Importation depuis Perforce ###

Dans le répertoire contrib/fast-import, vous trouverez le script git-p4.
C'est un script python qui importera un dépôt Perforce pour vous.

	$ ~/git.git/contrib/fast-import/git-p4 clone //depot/project/main@all myproject

### Importation depuis un autre SCM ###

Il y a d'autres SCMs listés en tête du Sondage Git, de la documentation est nécessaire
pour eux. !!TODO!!

* CVS
* Mercurial (hg)

* Bazaar-NG
* Darcs
* ClearCase
	
