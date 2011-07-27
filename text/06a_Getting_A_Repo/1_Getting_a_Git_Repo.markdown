## Obtenir un Dépôt Git ##

Maintenant que nous avons tout configuré, nous avons besoin d'un dépôt Git.
Nous pouvons faire ça de deux manières - soit en *clonant* un dépôt qui
existe déjà, soit en *initialisant* un dépôt depuis un dossier vide ou 
depuis des fichiers existants qui ne sont pas encore sous contrôle de version.

### Cloner un Dépôt ###

Afin d'obtenir une copie d'un projet, vous aurez besoin de connaître l'adresse
URL Git du projet - l'endroit où se trouve le dépôt. Git peut être utilisé
avec de nombreux protocoles, donc cette adresse peut commencer avec ssh://,
http(s)://, git://, ou juste un nom d'utilisateur (en supposant que git passe
par ssh). Par exemple, le code source de Git même peut être cloné en passant
par le protocole git :

    git clone git://git.kernel.org/pub/scm/git/git.git

ou par http :

    git clone http://www.kernel.org/pub/scm/git/git.git

Le protocole git:// est plus rapide et plus efficace, mais il est parfois
nécessaire d'utiliser http (derrière un firewall d'entreprise par exemple).
Dans tous les cas, vous devriez maintenant avoir un répertoire nommé 'git'
qui contient tout le code source de Git et son historique - c'est
simplement une copie de ce qui se trouvait sur le serveur.

Par défaut, Git nommera le nouveau répertoire, où il stockera votre code
cloné, en prenant ce qui arrive juste avant le '.git' dans le chemin
du projet cloné. (ie. *git clone
http://git.kernel.org/linux/kernel/git/torvalds/linux-2.6.git* créera un
nouveau répertoire nommé 'linux-2.6' pour y cloner le code).

### Initialiser un Nouveau Dépôt ###

Imaginons que nous avons une archive nommée project.tar.gz avec notre
travail initial. Nous pouvons le placer sous le contrôle de version de Git
comme ceci :

    $ tar xzf project.tar.gz
    $ cd project
    $ git init

Git vous répondra :

    Initialized empty Git repository in .git/

Vous avez maintenant initialisé le répertoire de travail et vous pourrez
y trouver un nouveau répertoire à l'intérieur, nommé ".git".

[gitcast:c1_init](GitCast #1 - setup, init and cloning)
