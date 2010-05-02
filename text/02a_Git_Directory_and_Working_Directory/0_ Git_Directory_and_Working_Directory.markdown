## Le répertoire Git et le répertoire de travail ##

### Le répertoire Git ###

Le « répertoire git » est un répertoire qui contient tout l’historique de Git
et les méta-informations du projet : tous les objets (commits, tree,
blobs, tags), tous les pointeurs vers les différentes branches et plus
encore.

Il y a un seul répertoire Git par projet (au contraire des systèmes comme SVN
ou CVS qui contiennent ce répertoire dans chaque sous-répertoire du projet).
Ce dossier se nomme '.git' (par défaut, il peut être nommé différemment) et
il se situe à la racine de votre projet. Si vous regardez le contenu
de ce répertoire, vous pouvez y trouver tous vos fichiers importants :

    $>tree -L 1
    .
    |-- HEAD         # pointeur vers votre branche courante
    |-- config       # configuration de vos préférences
    |-- description  # description de votre projet
    |-- hooks/       # pre/post action hooks
    |-- index        # fichier d'index (voir prochaine section)
    |-- logs/        # un historique de votre branche
    |-- objects/     # vos objets (commits, trees, blobs, tags)
    `-- refs/        # pointeurs vers vos branches

(Vous pourrez trouver d’autres fichiers/dossiers ici aussi, mais ils ne sont pas
 importants pour l’instant)

### Le répertoire de travail ###

Le « répertoire de travail » de Git est le répertoire qui contient la version
courante des fichiers sur lesquels vous travaillez. Les fichiers de ce répertoire
sont souvent effacés ou remplacés par Git quand vous changez de branche, ce qui est 
tout à fait normal. Tout votre historique est stocké dans votre répertoire Git ;
le répertoire de travail est simplement une version temporaire de votre projet 
dans lequel vous modifiez les fichiers jusqu'à votre prochain commit.