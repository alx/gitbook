## Personnaliser Git ##

linkgit:git-config[1]

### Changer l'Editeur ###

	$ git config --global core.editor emacs

### Ajouter des Alias ###
	
	$ git config --global alias.last 'cat-file commit HEAD'
	
	$ git last
	tree c85fbd1996b8e7e5eda1288b56042c0cdb91836b
	parent cdc9a0a28173b6ba4aca00eb34f5aabb39980735
	author Scott Chacon <schacon@gmail.com> 1220473867 -0700
	committer Scott Chacon <schacon@gmail.com> 1220473867 -0700

	fixed a weird formatting problem
	
	$ git cat-file commit HEAD
	tree c85fbd1996b8e7e5eda1288b56042c0cdb91836b
	parent cdc9a0a28173b6ba4aca00eb34f5aabb39980735
	author Scott Chacon <schacon@gmail.com> 1220473867 -0700
	committer Scott Chacon <schacon@gmail.com> 1220473867 -0700

	fixed a weird formatting problem

### Ajouter de la Couleur ###

Voir toutes les options color.* dans la documentation de linkgit:git-config[1]

	$ git config color.branch auto
	$ git config color.diff auto
	$ git config color.interactive auto
	$ git config color.status auto

Ou, vous pouvez toutes les activer avec l'option color.ui:

	$ git config color.ui true
	
### Template de Commit ###

	$ git config commit.template '/etc/git-commit-template'
	
### Format de Log ###

	$ git config format.pretty oneline


### Autres Options de Configuration ###

Il y a aussi un grand nombre d'options intéressantes pour archiver, nettoyer,
fusionner, créer des branches, pour le transport http, les diff, et plus.
Si vous voulez personnaliser toutes ces options, reportez-vous à
la documentation de linkgit:git-config[1].
