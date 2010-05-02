## Recombinaison interactive ##

Vous pouvez aussi recombiner (rebase) les commits interactivement.
Ceci s’utilise souvent pour ré-écrire vos objets commits avant de les publier.
C’est une manière simple de découper, regrouper et réordonner les commits
avant de les partager avec d'autres. Vous pouvez aussi utiliser ça pour
nettoyer les commits que vous récupérerez chez quelqu’un avant de les appliquer
localement.

Si vous voulez modifier interactivement vos commit durant la recombinaison,
vous pouvez activer le mode interactif en utilisant l’option `-i` ou
`--interactive` avec la commande `git rebase`.

	$ git rebase -i origin/master

Cela lancera le mode interactif de recombinaison avec tous les commits
que vous avez créés depuis votre dernière publication (ou la dernière
fusion depuis le dépôt d’origine).

Pour voir quels commits seront concernés, vous pouvez utiliser la
commande `log` de cette façon :
	
	$ git log github/master..

Quand vous lancerez la commande `rebase -i`, vous vous trouverez dans
un éditeur qui ressemblera à ça :

	pick fc62e55 added file_size
	pick 9824bf4 fixed little thing
	pick 21d80a5 added number to log
	pick 76b9da6 added the apply command
	pick c264051 Revert "added file_size" - not implemented correctly

	# Rebase f408319..b04dc3d onto f408319
	#
	# Commands:
	#  p, pick = use commit
	#  e, edit = use commit, but stop for amending
	#  s, squash = use commit, but meld into previous commit
	#
	# If you remove a line here THAT COMMIT WILL BE LOST.
	# However, if you remove everything, the rebase will be aborted.
	#

Cela signifie qu’il y a cinq commits depuis votre dernière publication et
chaque commit est décrit par un ligne avec le format suivant :

	(action) (sha partiel) (court message du commit)
	
Maintenant, vous pouvez changer l’action (qui est `pick` par défaut) soit
par `edit` ou `squash` ou juste la laisser comme elle est.
Vous pouvez aussi réordonner les commits en déplaçant les lignes comme
vous le voulez. Ensuite, quand vous sortez de l’éditeur, git essayera
d’appliquer les commits en suivant leur ordre d’arrangement et l’action
sélectionnée.

Si `pick` est sélectionné, il essayera simplement d’appliquer le patch et
de sauvegarder le commit avec le même message qu’avant.

Si `squash` est sélectionné, il combinera ce commit avec le précédent pour
former un nouveau commit. Vous trouverez alors un autre éditeur pour fusionner
les messages des deux commit qui ont été assemblés ensemble. Donc, si vous
sortez du premier éditeur de la manière suivante :

	pick   fc62e55 added file_size
	squash 9824bf4 fixed little thing
	squash 21d80a5 added number to log
	squash 76b9da6 added the apply command
	squash c264051 Revert "added file_size" - not implemented correctly

vous devrez créer un nouveau message de commit à partir de ça :

	# This is a combination of 5 commits.
	# The first commit's message is:
	added file_size

	# This is the 2nd commit message:

	fixed little thing

	# This is the 3rd commit message:

	added number to log

	# This is the 4th commit message:

	added the apply command

	# This is the 5th commit message:

	Revert "added file_size" - not implemented correctly

	This reverts commit fc62e5543b195f18391886b9f663d5a7eca38e84.

Une fois que vous aurez édité cette partie en un seul message et quitté
l’éditeur, le commit sera sauvegardé avec votre nouveau message.

Si vous avez sélectionné `edit`, la même chose se passera, mais une pause
sera marquée entre chaque commit pour vous donner la main avec une ligne de
commande afin que vous puissiez modifier le commit ou son contenu.

Par exemple, si vous voulez découper un commit, vous sélectionnerez `edit`
pour ce commit :

	pick   fc62e55 added file_size
	pick   9824bf4 fixed little thing
	edit   21d80a5 added number to log
	pick   76b9da6 added the apply command
	pick   c264051 Revert "added file_size" - not implemented correctly

quand vous vous trouverez avec la ligne de commande, vous pourrez revenir
sur ce commit pour en créer deux (ou plus) nouveaux. Disons que 21d80a5
modifie deux fichiers, fichier1 et fichier2 et que vous voulez le découper en
deux commits séparés. Vous pouvez faire ceci quand la recombinaison vous
redonne la main avec la ligne de commande :

	$ git reset HEAD^
	$ git add fichier1
	$ git commit 'première partie du commit découpé'
	$ git add fichier2
	$ git commit 'seconde partie du commit découpé'
	$ git rebase --continue
	
Et maintenant, au lieu d'avoir cinq commits, vous en avez six.

La recombinaison interactive peut vous aider sur un dernier point,
elle peut oublier des commits. Si au lieu de sélectionner `pick`, `squash`
ou `edit` pour la ligne de commit, vous effacez simplement la ligne, alors
le commit sera retiré de l’historique.
