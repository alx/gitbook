## Comment Git Stocke les Objets ##

Ce chapitre plonge dans les détails concernant le stockage physique
des objets avec Git.

Tous les objets sont stockés comme du contenu compressé par leur valeur SHA.
Il contiennent le type d'objet, la taille et le contenu au format gzip.

Git garde les objets dans 2 formats: les objets détendus et les objets
packagés.

### Les Objets Détendus ###

Le format le plus simple concerne les objets détendus. Ce ne sont que des
données comprimées stockées dans un seul fichier sur le disque. Chaque objet est
écrit dans un fichier séparé.

Si le SHA de l'objet est <code>ab04d884140f7b0cf8bbf86d6883869f16a46f65</code>,
alors le fichier sera stocké dans le chemin suivant:

	GIT_DIR/objects/ab/04d884140f7b0cf8bbf86d6883869f16a46f65

Il récupère les 2 premiers caractères et les utilisent comme sous-répertoire,
comme ça il n'y a jamais trop d'objets dans le même dossier. Le nom du fichier
est alors composé des 38 caractères restant.

En utilisant cette implémentation Ruby du stockage avec Git, nous pouvons examiner
simplement comment les données d'un objet sont stockées:

	ruby
	def put_raw_object(content, type)
	  size = content.length.to_s
 
	  header = "#{type} #{size}\0"
	  store = header + content
           
	  sha1 = Digest::SHA1.hexdigest(store)
	  path = @git_dir + '/' + sha1[0...2] + '/' + sha1[2..40]
 
	  if !File.exists?(path)
	    content = Zlib::Deflate.deflate(store)
 
	    FileUtils.mkdir_p(@directory+'/'+sha1[0...2])
	    File.open(path, 'w') do |f|
	      f.write content
	    end
	  end
	  return sha1
	end

### Les Objets Packagés ###

Le packfile est l'autre format de stockage d'objet. Depuis que Git stocke
chaque version de chaque fichier dans un objet séparé, il peu rapidement
devenir inefficace avec le format précédent Imaginez avoir un fichier long de
quelques milliers de lignes et si on change une ligne, git enregistrera un
second fichier dans son intégrité, cela serait un grand gaspillage d'espace.

Afin d'économiser cet espace, Git utilise le packfile. C'est une format où Git
ne va sauvegarder que la partie du fichier qui a changé dans le second
fichier, avec un pointeur vers le fichier similaire.

Quand les objets sont écrits sur le disque, le format détendu est souvent
utilisé, puisque ce format est plus simple d'accès. Cependant, vous
voudrez certainement sauver de l'espace en packageant les objets - cela
se fait avec la commande linkgit:git-gc[1]. Elle utilisera une heuristique
plutôt compliquée pour déterminer quels fichiers sont similaires et
basera les delta sur cette analyse. Nous pouvons avoir de multiple packfiles,
ils peuvent être re-packagés si nécessaire (linkgit:git-repack[1]) ou 
dé-packagés en de multiples fichiers (linkgit:git-unpack-objects[1])
relativement facilement.

Git écrira aussi un fichier d'index pour chaque packfile, il sera bien plus
petit et contiendra les offset du packfile pour trouver des objets spécifiques
plus rapidement en utilisant le SHA.

Le détail de l'implémentation du packfile se trouve dans le chapitre Packfile
disponible un peu plus loin dans ce livre.
