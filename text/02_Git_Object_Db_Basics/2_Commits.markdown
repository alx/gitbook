### L’objet commit ###

L’objet « commit » lie l’état physique d’un « tree » avec une description
de la manière et de la raison de l’arrivée à cet état.

[fig:object-commit]

Vous pouvez utiliser linkgit:git-show[1] ou linkgit:git-log[1] avec
l’option --pretty=raw pour examiner vos « commits » favoris :

    $ git show -s --pretty=raw 2be7fcb476
    commit 2be7fcb4764f2dbcee52635b91fedb1b3dcf7ab4
    tree fb3a8bdd0ceddd019615af4d57a53f43d8cee2bf
    parent 257a84d9d02e90447b149af58b271c19405edb6a
    author Dave Watson <dwatson@mimvista.com> 1187576872 -0400
    committer Junio C Hamano <gitster@pobox.com> 1187591163 -0700

        Fix misspelling of 'suppress' in docs

        Signed-off-by: Junio C Hamano <gitster@pobox.com>

Comme vous pouvez le voir, un commit est défini par :

- Un **« tree »** : Le nom SHA1 de l’objet « tree » (comme défini précédemment)
  représentant le contenu d’un répertoire à un certain moment.
- **parent(s)** : Le nom SHA1 du (ou des) numéro de « commits » qui représente(nt)
  l’étape antérieure dans l’historique du projet. L’exemple ci-dessus a un
  parent; les commits mergés peuvent en avoir plus d’un. Un « commit » sans parent
  est nommé le « commit racine » et représente la révision initiale d’un projet.
  Chaque projet doit contenir au moins une racine. Un projet peut avoir
  plusieurs racine, bien que ça ne soit pas très commun (ou que ça ne soit pas
  une bonne idée).
- Un **author** : Le nom de la personne responsable de ce changement,
  avec sa date
- Un **committer** : Le nom de la personne qui a créé le « commit », avec la date
  de création. Cet attribut peut être différent de l’auteur, par exemple, si
  l’auteur écrit un patch et l’envoi à une autre personne par mail, cet
  personne peut utilisé le patch pour créer le « commit ».
- Un **comment** qui décrit ce « commit ».

Notez qu’un « commit » ne contient pas d’information à propos de ce qui a été
modifié ; tous les changements sont calculés en comparant les contenus du
« tree » référencé dans ce « commit » avec le « tree » associé au(x) parent(s) du
« commit ». En particulier, git n’essaye pas d’enregistrer le renommage de fichier
explicitement, bien qu’il puisse identifier des cas où la persistance
des données d’un fichier avec un chemin modifié suggère un renommage (voir
par exemple, la commande linkgit:git-diff[1] avec l’option -M).

Un « commit » est normalement créé avec la commande linkgit:git-commit[1], qui
crée un « commit » dont le parent est le HEAD courant et avec le « tree » pris
depuis le contenu actuellement stocké dans l’index.

### Le modèle objet ###

Donc, maintenant que nous avons vu les 3 types d’objets principaux (blob, tree
et commit), regardons rapidement comment ils travaillent ensemble.

Si nous avons un simple projet avec la structure de dossiers suivante :

    $>tree
    .
    |-- README
    `-- lib
        |-- inc
        |   `-- tricks.rb
        `-- mylib.rb

    2 directories, 3 files

Et si nous committons ce projet sur un dépôt Git, il sera représenté comme ça :

[fig:objects-example]

Vous pouvez voir que nous avons créé un objet **tree** pour chaque répertoire (pour la
racine aussi) et un objet **blob** pour chaque fichier. Ensuite nous avons un objet
**commit** qui pointe vers la racine, afin que nous puissions récupérer l’apparence
du projet quand il a été committé.
