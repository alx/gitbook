## Ajout interactif ##

Il est vraiment intéressant de travailler et de visualiser l’index de Git avec
les ajouts interactifs. Pour le démarrer, tapez simplement `git add -i`. Git
vous montrera tous vos fichiers modifiés et leur état.

	$>git add -i
	           staged     unstaged path
	  1:    unchanged        +4/-0 assets/stylesheets/style.css
	  2:    unchanged      +23/-11 layout/book_index_template.html
	  3:    unchanged        +7/-7 layout/chapter_template.html
	  4:    unchanged        +3/-3 script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown

	*** Commands ***
	  1: status	  2: update	  3: revert	  4: add untracked
	  5: patch	  6: diff	  7: quit	  8: help
	What now> 

Dans ce cas, nous pouvons voir cinq fichiers modifiés qui n’ont pas encore été
ajoutés à l’index (`unstaged`) et le nombre de lignes en plus ou en moins pour
chacun d’eux. Nous trouvons aussi un menu interactif avec les possibilités de
cet outil.

Si nous voulons rajouter les fichiers dans l’index, nous pouvons taper `2`
ou `u` pour le mode de mise à jour (`update`). Après, nous devons sélectionner
les fichiers que nous voulons rajouter à l’index en saisissant les numéros
de ces fichiers (dans ce cas, 1-4) :

	What now> 2
	           staged     unstaged path
	  1:    unchanged        +4/-0 assets/stylesheets/style.css
	  2:    unchanged      +23/-11 layout/book_index_template.html
	  3:    unchanged        +7/-7 layout/chapter_template.html
	  4:    unchanged        +3/-3 script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	Update>> 1-4
	           staged     unstaged path
	* 1:    unchanged        +4/-0 assets/stylesheets/style.css
	* 2:    unchanged      +23/-11 layout/book_index_template.html
	* 3:    unchanged        +7/-7 layout/chapter_template.html
	* 4:    unchanged        +3/-3 script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	Update>> 

Si je presse « entrée », je reviendrais au menu principal où je peux voir que
l’état des fichiers a changé.

	What now> status
	           staged     unstaged path
	  1:        +4/-0      nothing assets/stylesheets/style.css
	  2:      +23/-11      nothing layout/book_index_template.html
	  3:        +7/-7      nothing layout/chapter_template.html
	  4:        +3/-3      nothing script/pdf.rb
	  5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown

Maintenant nous voyons que les quatre premiers fichiers sont assemblés et que le
dernier ne l’est pas encore. Ces informations sont simplement une compression
de l’affichage obtenu avec la commande `git status` :

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD <file>..." to unstage)
	#
	#	modified:   assets/stylesheets/style.css
	#	modified:   layout/book_index_template.html
	#	modified:   layout/chapter_template.html
	#	modified:   script/pdf.rb
	#
	# Changed but not updated:
	#   (use "git add <file>..." to update what will be committed)
	#
	#	modified:   text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	#

Il y a quelques options utiles dans ce mode d’ajout interactif, comme
retirer des fichiers de l’index (`3: revert`), ajouter des fichier non-suivis
(`4: add untracked`) et voir les différences (`6: view diff`). Ces options
sont assez faciles à comprendre. Cependant, il y a une commande plutôt
cool qui demande des explications : l’assemblage de patches (`5: patch`).

Si vous tapez `5` ou `p` dans le menu, git vous montrera les différences
patch par patch (ou morceau par morceau) et vous demandera si vous voulez
assembler chacun d’eux. De cette façon, vous pouvez n’assembler qu’une
partie d’un fichier modifié. Si vous avez édité un fichier et ne voulez
committer qu’une partie des modifications et laisser les autres modifications
dans un état inachevé ou séparer les commits de documentation de ceux des
changements plus conséquents, vous pouvez utiliser `git add -i` pour le 
faire facilement.

Ici j’ai assemblé quelques changements au fichier `book_index_template.html`,
mais pas tous :

	         staged     unstaged path
	1:        +4/-0      nothing assets/stylesheets/style.css
	2:       +20/-7        +3/-4 layout/book_index_template.html
	3:        +7/-7      nothing layout/chapter_template.html
	4:        +3/-3      nothing script/pdf.rb
	5:    unchanged      +121/-0 text/14_Interactive_Rebasing/0_ Interactive_Rebasing.markdown
	6:    unchanged       +85/-0 text/15_Interactive_Adding/0_ Interactive_Adding.markdown

Quand vous avez terminer de modifier votre index avec `git add -i`,
il vous suffit de quitter (`7: quit`) et de lancer `git commit` pour committer
les changements assemblés. Souvenez-vous de **ne pas lancer** `git commit -a`,
cela effacerait tous les précieuses sélections que vous avez faites dans le
mode interactif et committera tout d’un coup.

[gitcast:c3_add_interactive]("GitCast #3: Interactive Adding")
