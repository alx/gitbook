## Navigation dans les Objets Git ##

Nous pouvons demander à git des informations à propos d'un objet
particulier avec la commande cat-file. Vous pouvez utilise le SHA
partiel pour éviter de saisir les 40 caractères :

    $ git-cat-file -t 54196cc2
    commit
    $ git-cat-file commit 54196cc2
    tree 92b8b694ffb1675e5975148e1121810081dbdffe
    author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
    committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500

    initial commit

Un "tree" peut référencer un ou plusieurs objets "blob", chacun correspondant
à un fichier. En plus, un "tree" peut aussi référencer d'autres objets "tree",
créant ainsi une hiérarchie de dossier. Vous pouvez examiner le contenu
de n'importe quel "tree" en utilisant ls-tree (souvenez-vous qu'une portion
suffisamment longue du SHA1 fonctionnera aussi) :

    $ git ls-tree 92b8b694
    100644 blob 3b18e512dba79e4c8300dd08aeb37f8e728b8dad    file.txt

Nous pouvons voir que ce tree contient un fichier. Le hash SHA1 est une
référence aux données de ce fichier :

    $ git cat-file -t 3b18e512
    blob

Un "blob" n'est qu'un fichier de données, qui peut être examiné avec cat-file :

    $ git cat-file blob 3b18e512
    hello world

Notez qu'il s'agit ici des données de l'ancien fichier; donc l'objet que
git appelle dans sa réponse au "tree" initial était un "tree" avec une
capture de l'état du dossier qui a été enregistré par le premier commit.

Tous les objets sont stockés sous leur nom SHA1 dans le répertoire git :

    $ find .git/objects/
    .git/objects/
    .git/objects/pack
    .git/objects/info
    .git/objects/3b
    .git/objects/3b/18e512dba79e4c8300dd08aeb37f8e728b8dad
    .git/objects/92
    .git/objects/92/b8b694ffb1675e5975148e1121810081dbdffe
    .git/objects/54
    .git/objects/54/196cc2703dc165cbd373a65a4dcf22d50ae7f7
    .git/objects/a0
    .git/objects/a0/423896973644771497bdc03eb99d5281615b51
    .git/objects/d0
    .git/objects/d0/492b368b66bdabf2ac1fd8c92b39d3db916e59
    .git/objects/c4
    .git/objects/c4/d59f390b9cfd4318117afde11d601c1085f241

et le contenu de ces fichiers n'est que des données compressés avec
une entête identifiant leur type et leur taille. Le type peut être
un blob, un tree, un commit, ou un tag.

Le commit le plus simple à trouver est le commit HEAD, qui peut être
trouvé dans .git/HEAD :

    $ cat .git/HEAD
    ref: refs/heads/master

Comme vous pouvez le voir, ceci nous dit dans quelle branche nous nous
trouvons. Il nous le dit en nommant le fichier dans le répertoire
.git, qui lui-même contient le nom SHA1 référant à l'objet commit,
qui peut être examiné avec cat-file :

    $ cat .git/refs/heads/master
    c4d59f390b9cfd4318117afde11d601c1085f241
    $ git cat-file -t c4d59f39
    commit
    $ git cat-file commit c4d59f39
    tree d0492b368b66bdabf2ac1fd8c92b39d3db916e59
    parent 54196cc2703dc165cbd373a65a4dcf22d50ae7f7
    author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143418702 -0500
    committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143418702 -0500

    add emphasis

L'objet "tree" se réfère ici au nouvel état du "tree" :

    $ git ls-tree d0492b36
    100644 blob a0423896973644771497bdc03eb99d5281615b51    file.txt
    $ git cat-file blob a0423896
    hello world!

et l'objet "parent" se réfère au commit précédent :

    $ git-cat-file commit 54196cc2
    tree 92b8b694ffb1675e5975148e1121810081dbdffe
    author J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
    committer J. Bruce Fields <bfields@puzzle.fieldses.org> 1143414668 -0500
