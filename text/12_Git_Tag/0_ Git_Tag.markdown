## Tag avec Git ##

### Les Tags Légers ###

Vous pouvez créer un tag qui référence un commit particulier de git en lançant
linkgit:git-tag[1] en spécifiant le nom du tag et le nom SHA1 du commit
à tagger en options.

    $ git tag stable-1 1b2e1d63ff

Après ça, nous pouvons utiliser stable-1 pour faire référence au commit 1b2e1d63ff.

Ceci est un "tag léger", car aucune branche n'a été créée durant le processus.
Si vous voulez aussi rajouter un commentaire à ce tag, et le signer
avec une méthode cryptographique, alors nous allons plutôt créer un *objet tag*.

### Les Objets Tags ###

Si une de ces options : **-a**, **-s**, ou **-u <key-id>** est passée en argument
de la commande, alors nous créons un objet tag qui nécessite un message tag.
À moins que l'option -m <msg> ou -F <file> ne soit fournie, un éditeur se lance
pour que l'utilisateur saisisse le message du tag.

Quand cela se produit, un nouvel objet est ajouté à la base de données objet
de Git et la référence tag pointe vers cet _objet tag_, plutôt que sur le
commit lui-même. Ce concept est utile car vous pouvez maintenant signer le
tag, et vérifier plus tard que le commit correspond. Vous pouvez créer un
objet tag comme ceci :

    $ git tag -a stable-1 1b2e1d63ff

Il est possible de tagger n'importe quel objet, mais le taggage des objets
"commit" est la pratique la plus courante. (Dans les sources du kernel Linux,
le premier objet tag fait référence à un "tree", plutôt qu'à un "commit").

### Signature des Tags ###

Si vous avez configuré votre clé GPG, vous pouvez facilement créer des tags
signés. Dans un premier temps, vous devrez configurer l'identifiant de votre
clé dans votre fichier de configuration _.git/config_ ou _~.gitconfig_.

    [user]
        signingkey = <gpg-key-id>

Vous pouvez aussi le configurer comme ceci :

    $ git config (--global) user.signingkey <gpg-key-id>

Maintenant vous pouvez créer un tag signé en remplaçant juste le **-a**
par un **-s**.

    $ git tag -s stable-1 1b2e1d63ff

Si vous n'avez pas configuré une clé GPG dans votre fichier de configuration,
vous pouvez faire la même chose de cette façon :
    
    $ git tag -u <gpg-key-id> stable-1 1b2e1d63ff