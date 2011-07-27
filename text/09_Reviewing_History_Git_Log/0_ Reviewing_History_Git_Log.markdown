## Analyser l'Historique - Git Log ##

La commande linkgit:git-log[1] vous montrera des listes de commits.
Toute seule, elle vous montrera les commits visibles depuis le commit
parent de votre version courante ; mais vous pouvez faire des
requêtes plus spécifiques :

    $ git log v2.5..	    # commits depuis (non-visible depuis) v2.5
    $ git log test..master	# commits visibles depuis master mais pas test
    $ git log master..test	# commits visibles depuis test mais pas master
    $ git log master...test	# commits visibles pour test ou
    			            #    master, mais pas pour les 2
    $ git log --since="2 weeks ago" # commits des 2 dernières semaines
    $ git log Makefile      # commits modifiant le Makefile
    $ git log fs/		    # commits qui modifient les fichiers sous fs/
    $ git log -S'foo()'	    # commits qui ajoutent ou effacent des données
    			            #    contenant la chaîne 'foo()'
    $ git log --no-merges	# ne pas montrer les commits de merge

Et vous pouvez bien sûr combiner toutes ces options; la requête
suivante trouve tous les commits depuis v2.5 qui touchent le
Makefile et tous les fichier sous fs/ :

    $ git log v2.5.. Makefile fs/

Git log vous montrera la liste de chaque commit, le plus récent
d'abord, qui correspond aux arguments donnés à la commande de log.

	commit f491239170cb1463c7c3cd970862d6de636ba787
	Author: Matt McCutchen <matt@mattmccutchen.net>
	Date:   Thu Aug 14 13:37:41 2008 -0400

	    git format-patch documentation: clarify what --cover-letter does
    
	commit 7950659dc9ef7f2b50b18010622299c508bfdfc3
	Author: Eric Raible <raible@gmail.com>
	Date:   Thu Aug 14 10:12:54 2008 -0700

	    bash completion: 'git apply' should use 'fix' not 'strip'
	    Bring completion up to date with the man page.

Vous pouvez aussi demander à git de vous montrer les patches :

    $ git log -p

	commit da9973c6f9600d90e64aac647f3ed22dfd692f70
	Author: Robert Schiele <rschiele@gmail.com>
	Date:   Mon Aug 18 16:17:04 2008 +0200

	    adapt git-cvsserver manpage to dash-free syntax

	diff --git a/Documentation/git-cvsserver.txt b/Documentation/git-cvsserver.txt
	index c2d3c90..785779e 100644
	--- a/Documentation/git-cvsserver.txt
	+++ b/Documentation/git-cvsserver.txt
	@@ -11,7 +11,7 @@ SYNOPSIS
	 SSH:

	 [verse]
	-export CVS_SERVER=git-cvsserver
	+export CVS_SERVER="git cvsserver"
	 'cvs' -d :ext:user@server/path/repo.git co <HEAD_name>

	 pserver (/etc/inetd.conf):

### Statistique sur les Log ###

Vous pouvez rajouter l'options <code>--stat</code> à 'git log' pour
obtenir la liste des fichiers qui ont été changés dans un commit
et afficher combien de lignes ont été rajoutées ou retirées de chaque
fichier.

	$ git log --stat
	
	commit dba9194a49452b5f093b96872e19c91b50e526aa
	Author: Junio C Hamano <gitster@pobox.com>
	Date:   Sun Aug 17 15:44:11 2008 -0700

	    Start 1.6.0.X maintenance series
    
	 Documentation/RelNotes-1.6.0.1.txt |   15 +++++++++++++++
	 RelNotes                           |    2 +-
	 2 files changed, 16 insertions(+), 1 deletions(-)


### Formater le Log ###

Vous pouvez formater la sortie du log pour afficher ce que vous voulez.
L'option '--pretty' peut servir pour afficher les log avec un format
pré-réglé, comme 'oneline' :

	$ git log --pretty=oneline
	a6b444f570558a5f31ab508dc2a24dc34773825f dammit, this is the second time this has reverted
	49d77f72783e4e9f12d1bbcacc45e7a15c800240 modified index to create refs/heads if it is not 
	9764edd90cf9a423c9698a2f1e814f16f0111238 Add diff-lcs dependency
	e1ba1e3ca83d53a2f16b39c453fad33380f8d1cc Add dependency for Open4
	0f87b4d9020fff756c18323106b3fd4e2f422135 merged recent changes: * accepts relative alt pat
	f0ce7d5979dfb0f415799d086e14a8d2f9653300 updated the Manifest file

Ou vous pouvez utiliser le format 'short' :

	$ git log --pretty=short
	commit a6b444f570558a5f31ab508dc2a24dc34773825f
	Author: Scott Chacon <schacon@gmail.com>

	    dammit, this is the second time this has reverted

	commit 49d77f72783e4e9f12d1bbcacc45e7a15c800240
	Author: Scott Chacon <schacon@gmail.com>

	    modified index to create refs/heads if it is not there

	commit 9764edd90cf9a423c9698a2f1e814f16f0111238
	Author: Hans Engel <engel@engel.uk.to>

	    Add diff-lcs dependency

Vous pouvez aussi utiliser 'medium', 'full', 'fuller', 'email' ou 'raw'. Si ces
formats ne sont pas exactement ce dont vous avez besoin, vous pouvez créer votre
propre format avec l'option '--pretty=format' (voir la documentation de
linkgit:git-log[1] pour toutes les options de formatage).

	$ git log --pretty=format:'%h was %an, %ar, message: %s'
	a6b444f was Scott Chacon, 5 days ago, message: dammit, this is the second time this has re
	49d77f7 was Scott Chacon, 8 days ago, message: modified index to create refs/heads if it i
	9764edd was Hans Engel, 11 days ago, message: Add diff-lcs dependency
	e1ba1e3 was Hans Engel, 11 days ago, message: Add dependency for Open4
	0f87b4d was Scott Chacon, 12 days ago, message: merged recent changes:
	
Autre chose intéressante, vous pouvez aussi visualiser le graphe des commits
avec l'options '--graph', comme ceci :

	$ git log --pretty=format:'%h : %s' --graph
	* 2d3acf9 : ignore errors from SIGCHLD on trap
	*   5e3ee11 : Merge branch 'master' of git://github.com/dustin/grit
	|\  
	| * 420eac9 : Added a method for getting the current branch.
	* | 30e367c : timeout code and tests
	* | 5a09431 : add timeout protection to grit
	* | e1193f8 : support for heads with slashes in them
	|/  
	* d6016bc : require time for xmlschema

Ça vous montrera un représentation plutôt réussie des lignes de
l'historique des commits.

### Ordonner le Log ###

Vous pouvez aussi voir les entrées de log dans des ordres différents.
Notez que git log commence par le commit le plus récent et va
à reculons vers ses parents; cependant, puisque l'historique de git
peut contenir de multiples lignes indépendantes de développement,
l'ordre de l'affiche des commits est plutôt arbitraire.

Si vous voulez spécifier un ordre en particulier, vous pouvez ajouter
une option à la commande git log.

Par défaut les commit sont montrés dans l'ordre inversement
chronologique.

Cependant, si vous ajoutez l'option '--topo-order', les commits
apparaîtrons dans l'ordre topologique (i.e. les commits descendants
sont affichés avant leurs parents). Si nous regardons le git log
pour le dépôt de Grit dans un ordre topologique, vous pouvez voir
que toutes les lignes de développement sont regroupées ensemble.

	$ git log --pretty=format:'%h : %s' --topo-order --graph
	*   4a904d7 : Merge branch 'idx2'
	|\  
	| *   dfeffce : merged in bryces changes and fixed some testing issues
	| |\  
	| | * 23f4ecf : Clarify how to get a full count out of Repo#commits
	| | *   9d6d250 : Appropriate time-zone test fix from halorgium
	| | |\  
	| | | * cec36f7 : Fix the to_hash test to run in US/Pacific time
	| | * | decfe7b : fixed manifest and grit.rb to make correct gemspec
	| | * | cd27d57 : added lib/grit/commit_stats.rb to the big list o' files
	| | * | 823a9d9 : cleared out errors by adding in Grit::Git#run method
	| | * |   4eb3bf0 : resolved merge conflicts, hopefully amicably
	| | |\ \  
	| | | * | d065e76 : empty commit to push project to runcoderun
	| | | * | 3fa3284 : whitespace
	| | | * | d01cffd : whitespace
	| | | * | 7c74272 : oops, update version here too
	| | | * | 13f8cc3 : push 0.8.3
	| | | * | 06bae5a : capture stderr and log it if debug is true when running commands
	| | | * | 0b5bedf : update history
	| | | * | d40e1f0 : some docs
	| | | * | ef8a23c : update gemspec to include the newly added files to manifest
	| | | * | 15dd347 : add missing files to manifest; add grit test
	| | | * | 3dabb6a : allow sending debug messages to a user defined logger if provided; tes
	| | | * | eac1c37 : pull out the date in this assertion and compare as xmlschemaw, to avoi
	| | | * | 0a7d387 : Removed debug print.
	| | | * | 4d6b69c : Fixed to close opened file description.

Vous pouvez aussi utiliser '--date-order', qui ordonne les commits par date.
Cette option est similaire à '--topo-order' dans le sens où les parents seront affichés après
tus leurs enfants, mais autrement les commits sont toujours ordonnés suivant la date. Vous
pouvez voir ici que les lignes de développement sont groupées ensemble, et qu'elle
s'éloignent quand un développement parallèle à lieu :

	$ git log --pretty=format:'%h : %s' --date-order --graph
	*   4a904d7 : Merge branch 'idx2'
	|\  
	* | 81a3e0d : updated packfile code to recognize index v2
	| *   dfeffce : merged in bryces changes and fixed some testing issues
	| |\  
	| * | c615d80 : fixed a log issue
	|/ /  
	| * 23f4ecf : Clarify how to get a full count out of Repo#commits
	| *   9d6d250 : Appropriate time-zone test fix from halorgium
	| |\  
	| * | decfe7b : fixed manifest and grit.rb to make correct gemspec
	| * | cd27d57 : added lib/grit/commit_stats.rb to the big list o' file
	| * | 823a9d9 : cleared out errors by adding in Grit::Git#run method
	| * |   4eb3bf0 : resolved merge conflicts, hopefully amicably
	| |\ \  
	| * | | ba23640 : Fix CommitDb errors in test (was this the right fix?
	| * | | 4d8873e : test_commit no longer fails if you're not in PDT
	| * | | b3285ad : Use the appropriate method to find a first occurrenc
	| * | | 44dda6c : more cleanly accept separate options for initializin
	| * | | 839ba9f : needed to be able to ask Repo.new to work with a bar
	| | * | d065e76 : empty commit to push project to runcoderun
	* | | | 791ec6b : updated grit gemspec
	* | | | 756a947 : including code from github updates
	| | * | 3fa3284 : whitespace
	| | * | d01cffd : whitespace
	| * | | a0e4a3d : updated grit gemspec
	| * | | 7569d0d : including code from github updates

Enfin, vous pouvez inverser l'ordre du log avec l'option '--reverse'.

[gitcast:c4-git-log]("GitCast #4: Git Log")
