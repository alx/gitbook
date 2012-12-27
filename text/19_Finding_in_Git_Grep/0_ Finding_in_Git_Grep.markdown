## Recherche avec Git Grep ##

Il est très facile de trouver des fichiers avec des mots ou des phrases
avec la commande linkgit:git-grep[1]. Il est aussi possible de le faire
avec la commande 'grep' unix, mais vous pouvez faire des recherches sur
vos versions précédentes avec 'git grep' sans avoir à les rapatrier.

Par exemple, si je veux voir tous les endroits qui appellent 'xmmap' dans
mon dépôt git.git, je pourrai lancer ça:

	$ git grep xmmap
	config.c:               contents = xmmap(NULL, contents_sz, PROT_READ,
	diff.c:         s->data = xmmap(NULL, s->size, PROT_READ, MAP_PRIVATE, fd, 0);
	git-compat-util.h:extern void *xmmap(void *start, size_t length, int prot, int fla
	read-cache.c:   mmap = xmmap(NULL, mmap_size, PROT_READ | PROT_WRITE, MAP_PRIVATE,
	refs.c: log_mapped = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, logfd, 0);
	sha1_file.c:    map = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, fd, 0);
	sha1_file.c:    idx_map = xmmap(NULL, idx_size, PROT_READ, MAP_PRIVATE, fd, 0);
	sha1_file.c:                    win->base = xmmap(NULL, win->len,
	sha1_file.c:                    map = xmmap(NULL, *size, PROT_READ, MAP_PRIVATE, f
	sha1_file.c:            buf = xmmap(NULL, size, PROT_READ, MAP_PRIVATE, fd, 0);
	wrapper.c:void *xmmap(void *start, size_t length,

Si je veux aussi voir le numéro de ligne de chacun de ces résultats, je peux
ajouter l'option '-n':

	$>git grep -n xmmap
	config.c:1016:          contents = xmmap(NULL, contents_sz, PROT_READ,
	diff.c:1833:            s->data = xmmap(NULL, s->size, PROT_READ, MAP_PRIVATE, fd,
	git-compat-util.h:291:extern void *xmmap(void *start, size_t length, int prot, int
	read-cache.c:1178:      mmap = xmmap(NULL, mmap_size, PROT_READ | PROT_WRITE, MAP_
	refs.c:1345:    log_mapped = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, logfd, 0);
	sha1_file.c:377:        map = xmmap(NULL, mapsz, PROT_READ, MAP_PRIVATE, fd, 0);
	sha1_file.c:479:        idx_map = xmmap(NULL, idx_size, PROT_READ, MAP_PRIVATE, fd
	sha1_file.c:780:                        win->base = xmmap(NULL, win->len,
	sha1_file.c:1076:                       map = xmmap(NULL, *size, PROT_READ, MAP_PR
	sha1_file.c:2393:               buf = xmmap(NULL, size, PROT_READ, MAP_PRIVATE, fd
	wrapper.c:89:void *xmmap(void *start, size_t length,

Si nous sommes seulement intéressé par le nom du fichier, nous pouvons utiliser
l'option '--name-only':

	$>git grep --name-only xmmap
	config.c
	diff.c
	git-compat-util.h
	read-cache.c
	refs.c
	sha1_file.c
	wrapper.c

Nous pouvons aussi voir combien de lignes correspondent dans chaque fichier
avec l'option '-c':

	$>git grep -c xmmap
	config.c:1
	diff.c:1
	git-compat-util.h:1
	read-cache.c:1
	refs.c:1
	sha1_file.c:5
	wrapper.c:1

Maintenant, si je veux voir dans quelle version de git ce code a été utilisé,
je pourrai ajouter la référence du tag à la fin, comme ceci:

	$ git grep xmmap v1.5.0
	v1.5.0:config.c:                contents = xmmap(NULL, st.st_size, PROT_READ,
	v1.5.0:diff.c:          s->data = xmmap(NULL, s->size, PROT_READ, MAP_PRIVATE, fd,
	v1.5.0:git-compat-util.h:static inline void *xmmap(void *start, size_t length,
	v1.5.0:read-cache.c:                    cache_mmap = xmmap(NULL, cache_mmap_size, 
	v1.5.0:refs.c:  log_mapped = xmmap(NULL, st.st_size, PROT_READ, MAP_PRIVATE, logfd
	v1.5.0:sha1_file.c:     map = xmmap(NULL, st.st_size, PROT_READ, MAP_PRIVATE, fd, 
	v1.5.0:sha1_file.c:     idx_map = xmmap(NULL, idx_size, PROT_READ, MAP_PRIVATE, fd
	v1.5.0:sha1_file.c:                     win->base = xmmap(NULL, win->len,
	v1.5.0:sha1_file.c:     map = xmmap(NULL, st.st_size, PROT_READ, MAP_PRIVATE, fd, 
	v1.5.0:sha1_file.c:             buf = xmmap(NULL, size, PROT_READ, MAP_PRIVATE, fd

Nous pouvons voir les différences entre le code en cours et la version 1.5.0.
Dans l'une d'elles, xmmap est maintenant utilisé dans wrapper.c alors qu'il
ne l'était pas sans la v1.5.0.

Vous pouvez aussi combiner les termes de recherche dans grep. Par exemple,
si nous cherchons où est définie SORT_DIRENT dans notre dépôt:

	$ git grep -e '#define' --and -e SORT_DIRENT
	builtin-fsck.c:#define SORT_DIRENT 0
	builtin-fsck.c:#define SORT_DIRENT 1

Vous pouvez aussi rechercher tous les fichiers qui contiennent la *totalité*
des termes recherchés, et affiche les lignes *avec l'un de* ces 2 termes:

	$ git grep --all-match -e '#define' -e SORT_DIRENT
	builtin-fsck.c:#define REACHABLE 0x0001
	builtin-fsck.c:#define SEEN      0x0002
	builtin-fsck.c:#define ERROR_OBJECT 01
	builtin-fsck.c:#define ERROR_REACHABLE 02
	builtin-fsck.c:#define SORT_DIRENT 0
	builtin-fsck.c:#define DIRENT_SORT_HINT(de) 0
	builtin-fsck.c:#define SORT_DIRENT 1
	builtin-fsck.c:#define DIRENT_SORT_HINT(de) ((de)->d_ino)
	builtin-fsck.c:#define MAX_SHA1_ENTRIES (1024)
	builtin-fsck.c: if (SORT_DIRENT)

Vous pouvez aussi rechercher les lignes qui ont un terme et chacun de
ces 2 termes, par exemple, si vous voulez voir où sont définies les
constantes qui ont soit PATH ou MAX dans leur nom:

	$ git grep -e '#define' --and \( -e PATH -e MAX \) 
	abspath.c:#define MAXDEPTH 5
	builtin-blame.c:#define MORE_THAN_ONE_PATH      (1u<<13)
	builtin-blame.c:#define MAXSG 16
	builtin-describe.c:#define MAX_TAGS     (FLAG_BITS - 1)
	builtin-fetch-pack.c:#define MAX_IN_VAIN 256
	builtin-fsck.c:#define MAX_SHA1_ENTRIES (1024)
	...
