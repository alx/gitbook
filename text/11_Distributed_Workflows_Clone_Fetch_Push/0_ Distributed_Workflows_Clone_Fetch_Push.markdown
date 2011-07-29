## Workflows Distribués ##

Supposons qu'Alice ait démarré un nouveau projet dans son dépôt git
situé dans /home/alice/project, et que Bob, qui a un répertoire
utilisateur sur la même machine (/home/bob/), veuille y
contribuer.

Bob commence par :

    $ git clone /home/alice/project mondepot

Cela crée un répertoire "mondepot" qui contient un clone du dépôt
d'Alice. Le clone est une copie parfaite du projet original,
possédant aussi sa propre copie de l'historique du projet original.

Bob fait quelques changements et les commit :

    (editer des fichiers)
    $ git commit -a
    (répéter autant que nécessaire)

Quand il est prêt, il dit à Alice de récupérer (pull) ses changements
depuis son dépôt situé dans /home/bob/mondepot. Alice fait alors :

    $ cd /home/alice/project
    $ git pull /home/bob/myrepo master

Cela merge les changements de la branche "master" de Bob dans la branche
courante d'Alice. Si Alice a fait ses propres changements pendant ce temps,
alors elle devra peut être réparer quelques conflit à la main. (Notez l'option
"master" dans la commande ci-dessus, elle n'est pas nécessaire car c'est
l'option par défaut.)

La commande "pull" travaille donc en deux étapes : elle récupère les changements
d'une branche distante, et merge ces changements dans la branche courante.

Quand vous travaillez dans une petite équipe soudée, il est courant
que tous interagissent très souvent avec le même dépôt. En définissant un
raccourci pour le dépôt 'distant', nous pouvons rendre ces opérations plus
simples :

    $ git remote add bob /home/bob/mondepot

Avec ça, Alice peut effectuer la première opération seulement en
utilisant la commande "git fetch", elle ne mergera pas les modifications
avec sa propre branche :

    $ git fetch bob

Contrairement à la version longue, quand Alice récupère (fetch) les
données de Bob en utilisant un raccourci configuré avec 'git remote',
alors ce qui est récupéré est stocké dans une branche de suivi distante,
dans notre cas 'bob/master'. Donc maintenant :

    $ git log -p master..bob/master

montre la liste de tous les changements que Bob a fait depuis qu'il a créé
une branche depuis la branche "master" d'Alice.

Après avoir examiné ces changements, Alice peut les merger dans sa branche
"master" :

    $ git merge bob/master

Ce `merge` peut aussi être fait en récupérant les données depuis
sa propre branche de suivi distant, comme ceci :

    $ git pull . remotes/bob/master

Git récupère toujours les merges dans la branche courante,
quelles que soient les options de la ligne de commande.

Plus tard, Bob peut mettre à jour son dépôt avec les dernières
modifications d'Alice en utilisant :

    $ git pull

Il n'a pas besoin de donner le chemin vers le dépôt d'Alice ; quand Bob a cloné
le dépôt d'Alice, git a stocké l'adresse de son dépôt dans la configuration
du dépôt, et cette adresse est utilisée pour récupérer les données avec 'pull' :

    $ git config --get remote.origin.url
    /home/alice/project

(La configuration complète créée par git-clone est visible en lançant
"git config -l", et la page de documentation de linkgit:git-config[1]
explique chacune de ces options.)

Git conserve aussi une copie propre de la branche "master" d'Alice,
sous le nom "origin/master" :

    $ git branch -r
      origin/master

Si Bob décide plus tard de travailler avec un hébergeur différent,
il pourra toujours créer des clones et récupérer les données en utilisant
le protocole ssh :

    $ git clone alice.org:/home/alice/project myrepo

D'une autre manière, git contient un protocole natif, ou peut aussi utiliser
rsync ou http ; voir linkgit:git-pull[1] pour plus de détails.

Git peut aussi être utilisé de manière plus similaire à CVS, avec un dépôt
central sur lequel de nombreux utilisateurs envoient leur modifications ;
voir linkgit:git-push[1] et linkgit:gitcvs-migration[1].

### Les Dépôts Git Publiques ###

Une autre façon d'envoyer des modifications à un projet est d'avertir
le chef de ce projet afin qu'il récupère les changements depuis votre
dépôt en utilisant linkgit:git-pull[1]. C'est une manière d'obtenir les
mises à jour du dépôt principal, mais cela fonctionne aussi dans
l'autre sens.

Si vous et le chef de projet avez tous les deux un compte sur le même
ordinateur, alors vous pouvez échanger les modifications de vos dépôts
respectifs directement ; les commandes qui acceptent des URL de dépôts
comme options, accepteront aussi un chemin de répertoire local :

    $ git clone /path/to/repository
    $ git pull /path/to/other/repository

ou une adresse ssh :

    $ git clone ssh://yourhost/~you/repository

Pour les projets avec quelques développeurs, ou pour synchroniser quelques
projets privés, cela peut vous suffire.

Cependant, la pratique la plus courante est de maintenir un dépôt publique
(généralement sur le même host) pour que les autres puissent y récupérer les
changements. Cela est souvent plus efficace, et vous permet de séparer
proprement le travail privé en cours de réalisation des projets publiques
et visibles.

Vous continuerez à travailler au jour-le-jour sur votre dépôt personnel,
mais périodiquement vous enverrez (push) les modifications de votre
dépôt personnel sur votre dépôt publique, permettant alors aux autres
développeurs de récupérer (pull) les changements disponibles dans ce dépôt.
Donc le flux de travail, dans une situation où un autre développeur 
fournit des changements dans son dépôt publique, ressemble à ça :

                               vous envoyez (push)
      votre dépôt personnel ---------------------------> votre dépôt publique
                ^                                               |
                |                                               |
                | vous récupérez (pull)                         | ils récupèrent (pull)
                |                                               |
                |                                               |
                |               ils envoient (push)             V
      leur dépôt publique <------------------------------  leur dépôt
      


### Publier des Modifications sur un Dépôt Publique ###

L'utilisation des protocoles http et git permet aux autres développeurs
de récupérer les derniers changements, mais ils n'auront pas l'autorisation
d'écriture sur ces dépôt. Pour cela, vous devrez mettre à jour votre dépôt
publique avec les derniers changements de votre dépôt privé.

La façon la plus simple de procéder est d'utiliser linkgit:git-push[1] et ssh ;
pour mettre à jour la branche "master" avec le dernier état de votre branche
"master", lancez :

    $ git push ssh://yourserver.com/~you/proj.git master:master

ou juste

    $ git push ssh://yourserver.com/~you/proj.git master

Comme avec git-fetch, git-push se plaindra si l'opération ne se résume
pas à une avance rapide (fast-forward) ; allez à la section suivante
pour plus de détails pour gérer ce cas.

La cible d'un "push" est normalement un dépôt 'nu'. Vous pouvez aussi publier
vers un dépôt qui contient une arborescence de travail, mais cette arborescence
ne sera pas mise à jour durant la publication. Cela pourra vous amener à des
résultats inattendus, par exemple si la branche que vous publiez est l'actuelle
branche de travail sur le dépôt publique.

Comme avec git-fetch, vous pouvez aussi rajouter des options de configuration
pour vous permettre d'aller plus rapidement ; par exemple, après :

    $ cat >>.git/config <<EOF
    [remote "public-repo"]
    	url = ssh://yourserver.com/~you/proj.git
    EOF

vous devriez pouvoir effectuer les opérations précédentes avec juste

    $ git push public-repo master

Voir les explications des options remote.<name>.url, branch.<name>.remote,
et remote.<name>.push dans linkgit:git-config[1] pour plus de détails.

### Que faire quand une publication échoue ###

Si une publication ne se termine pas avec une avance de la branche distante,
alors elle échouera avec un message d'erreur comme celui-ci :

    error: remote 'refs/heads/master' is not an ancestor of
    local  'refs/heads/master'.
    Maybe you are not up-to-date and need to pull first?
    error: failed to push to 'ssh://yourserver.com/~you/proj.git'

Cela peut arriver, par exemple, si :

	- vous avez utilisé `git-reset --hard` pour effacer un commit déjà publié, ou
	- vous avez utilisé `git-commit --amend` pour remplacer un commit déjà publié, ou
	- vous avez utilisé `git-rebase` pour recombiner un commit déjà publié.

Vous pouvez forcer un git-push à effectuer quand même la mise à jour
en rajoutant le préfixe '+' au nom de la branche :

    $ git push ssh://yourserver.com/~you/proj.git +master

Normalement, quand le sommet de la branche d'un dépôt public est modifié,
il est modifié pour pointer vers un descendant du commit vers lequel il
pointait avant. En forçant la publication dans cette situation, vous
cassez cette convention.

Néanmoins, c'est une bonne pratique pour les gens qui ont besoin de publier
simplement une série de patches des travaux en cours, et c'est un
compromis acceptable tant que vous prévenez les autres développeurs que
vous comptez gérer la branche de cette façon.

Il est aussi possible qu'une publication échoue de cette façon quand
d'autres personnes ont le droit de publier sur le même dépôt. Dans ce cas,
la solution la plus correcte est de retenter la publication après avoir mis
à jour votre travail : soit par un git-pull, soit par un git-fetch suivi
d'une recombinaison (rebase) ; voir la prochaine partie et
linkgit:gitcvs-migration[7] pour plus de détails.

[gitcast:c8-dist-workflow]("GitCast #8: Distributed Workflow")
