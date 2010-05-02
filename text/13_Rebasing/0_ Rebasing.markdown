## Recombinaison (rebase) ##

Supposons que vous avez créé une branche `mywork` sur une branche
de suivi distante `origin`.

    $ git checkout -b mywork origin

[fig:rebase0]

Maintenant travaillez un peu dessus, en créant 2 commits :

    $ vi fichier.txt
    $ git commit
    $ vi autrefichier.txt
    $ git commit
    ...

Pendant ce temps, quelqu’un d’autre travaille en créant aussi deux nouveaux
commits sur la branche d’origine. Cela signifie que les deux branches
`origine` et `mywork` ont avancées et elles ont aussi divergées.

[fig:rebase1]

À ce moment, vous pouvez utiliser `pull` pour fusionner vos modifications,
le résultat créera un nouveau commit `merge`, comme ceci :

[fig:rebase2]

Cependant, si vous préférez garder l’historique de `mywork` sous
l’aspect d’une simple série de commits sans `merge`, vous pouvez aussi
utiliser linkgit:git-rebase[1] :

    $ git checkout mywork
    $ git rebase origin

Cette commande va retirer chacun de vos commit sur `mywork` en les
sauvegardant temporairement comme des patches (dans le dossier `.git/rebase`),
puis mettre à jour la branche `mywork` avec la dernière version de la
branche `origin` et enfin appliquer chaque patch sauvegardé à cette nouvelle
version de `mywork`.

[fig:rebase3]

Une fois que la référence (`mywork`) est mise à jour jusqu'au dernier objet
commit créé, vos anciens commits seront abandonnés. Ils seront sûrement
effacés si vous lancez la commande de ramasse-miettes (voir linkgit:git-gc[1]).

[fig:rebase4]

Maintenant nous pouvons voir la différence de l’historique entre l’exécution
d’une fusion et l’exécution d'une recombinaison (rebase) :

[fig:rebase5]

Dans le processus d’une recombinaison, des conflits peuvent se produire.
Dans ce cas, le processus s’arrêtera et vous permettra de réparer ces conflits.
Après les avoir résolus, utilisez `git-add` pour mettre à jour l’index avec ce
nouveau contenu, puis, au lieu de lancer `git-commit`, lancez juste :

    $ git rebase --continue

et git continuera d’appliquer le reste des patches.

À n’importe quel moment, vous pouvez utiliser l’option `--abort` pour
annuler le processus et retourner au même état de `mywork` qu’au
démarrage de la recombinaison :

    $ git rebase --abort


[gitcast:c7-rebase]("GitCast #7: Rebasing")
