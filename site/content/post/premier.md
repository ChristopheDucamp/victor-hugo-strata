---
title: "Victor Hugo sur Netlify : Un Guide étape par étape"
date: 2017-07-03T20:21:25+02:00
draft: false
---

[Source Netlify](https://www.netlify.com/blog/2016/09/21/a-step-by-step-guide-victor-hugo-on-netlify/ "Permalink to A Step-by-Step Guide: Victor-Hugo on Netlify") - Eli  Williamson

# Victor-Hugo sur Netlify : Un Guide Étape par Étape

Jetons un oeil aujourd'hui sur la façon d'héberger un site web statique utilisant [Hugo][1] sur Netlify, en n'oubliant pas d'installer un déploiement continu.

Dans ce didacticiel, nous utiliserons [Victor Hugo](https://github.com/netlify/victor-hugo) (un modèle Hugo continuellement maintenu) pour construire notre site statique.

Pour commencer, nous allons nous assurer que vous disposez de tous les outils dont vous aurez besoin.
Allez-y et téléchargez Victor Hugo [ici](https://github.com/netlify/victor-hugo).

Si vous avez déjà installé un site Hugo, vous pouvez passer directement en à la section "Connexion à Netlify" en bas de la page.

## Construire votre Site

Hugo construit et organise un squelette pour votre site, qui installera un répertoire principal ; pour simplifier, nous appellerons ça `hugo`. En outre, il configure des sous-répertoires dans ce dossier `hugo` comme décrit ci-dessous. Voici une ventilation basique de la structure que Victor Hugo construit pour vous :


    |--site                // Tout ce qui est dedans sera construit avec hugo
    |  |--content          // Pages et collections. demande si pages supplémentaires
    |  |--data             // Fichiers data YAML avec toute data à utiliser dans les exemples
    |  |--layouts          // C’est l’endroit où vont les templates
    |  |  |--partials      // C’est là où vivent les includes
    |  |  |--index.html    // La page index
    |  |--static           // Les fichiers placés ici terminent dans le répertoire public
    |  |--config.toml      // L’endroit où vous réglez/stockez vos réglages de configuration
    |--src                 // Les fichiers qui passeront à travers le pipeline asset
    |  |--css              // Les fichiers CSS à la racine de ce répertoire atterrissent dans /css/...
    |  |--js               // Les app.js seront compilées dans /js/app.js avec babel


Pour démarrer, ouvrez la fenêtre de terminal, naviguez à l’endroit où réside votre répertoire Victor Hugo :

    $ cd /PATH/TO/hugo


Puis installez les dépendances nécessaires en saisissant :


    $ npm install    

Pour démarrer le serveur, saisissez :

    $ npm start    

à ce stade, votre navigateur devrait s'ouvrir et afficher une page de titre

> Hugo with Gulop, PostCSS et Webpack

## Créons une page À Propos

Dans le terminal, (arrêter le serveur au besoin en tapant Ctrl+C), naviguez vers le répertoire `site` ce qui peut être fait à partir du terminal avec la ligne de commande :


    $ cd site

Désormais, dirigez-vous directement et initialisez la page about en utilisant la commande Hugo suivante :


    $ hugo new a-propos.md

Si vous ouvrez le répertoire `hugo` dans votre gestionnaire de fichiers, vous verrez une liste de sous-répertoires. Ouvrez le fichier `site/content/a-propos.md` pour voir ce que vous venez de créer. Vous devriez voir quelque chose comme suit :

```bash
    ---
    title: "A Propos"
    date: 2017-07-03T19:15:58+02:00
    draft: true
    ---
```

Placez un peu de contenu en format Markdown sous le front-matter (la section marquée entre les signes `---` avant et après) et sauvegardez.

## Maintenant, ajoutons un nouveau post.

Pour faire ceci, entrez ce qui suit :

    $ hugo new post/premier.md    

Le nouveau fichier est situé sur `content/post/premier.md`. Il attend juste d’être un peu épicé, non ?

```bash

---
title: "Premier"
date: 2017-07-03T19:20:37+02:00
draft: true

---
> «La paix de l'esprit produit des valeurs justes, les valeurs justes produisent des pensées droites. Les bonnes pensées produisent des actions justes et les actions justes produisent un travail qui sera une réflexion matérielle pour que les autres puissent voir la sérénité au centre de tout cela.

    ― Robert M. Pirsig, Zen and the Art of Motorcycle Maintenance

```

**Truc :** Créez quelques posts comme celui-ci pour avoir du contenu en place et regarder la mise en page de plusieurs articles sous forme de listes.

Maintenant que vous avez du contenu, il est temps de l'afficher. Hugo dispose d'un [dossier plein de thèmes][4] que vous pouvez utiliser (ou vous pouvez toujours créer un thème personnalisé). Pour les besoins de ce tutoriel, utilisons le thème [Strata](https://github.com/digitalcraftsman/hugo-strata-theme).

Retrouvez le thème Strata dans la bibliothèque de thèmes  Hugo (ou cliquez [ici][5]). Pour éviter les maux de tête et vous faciliter la vie sur la modification du thème, nous allons simplement télécharger le zip au lieu de cloner le repo du thème. Pour ce faire, cliquez sur le bouton **Clone or download**, puis cliquez sur le bouton **Download zip** situé à droite de la page. Dans le dossier `hugo/site`, créez un nouveau dossier appelé `themes`, puis décompressez le fichier Strata dans `/VotreChemin/VERS/hugo/themes`. N'oubliez pas de renommer le dossier de `hugo-strata-theme-master` en un simple `hugo-strata-theme`.

## Installer votre premier thème

La plupart des thèmes documentent la manière de les régler dans leur fichier `README.md`. Pour le thème Strata, il y a cinq choses que nous devons faire.

1. Pour éviter les conflits avec le thème, nous allons effacer quelques-uns des fichiers initiaux, aussi effacez tous les fichiers dans le répertoire `layouts` situé sur `site/layouts`. Un autre fichier de conflit est `src/css/main.css` - effacez aussi ce fichier. Si vous construisez votre propre thème, ne tenez pas compte de ces étapes.

2. Puis nous devons copier les contenus du fichier exemple `config.toml` à partir de `themes/hugo-strata-theme/exampleSite` à l'intérieur de notre fichier du site `config.toml` situé sur `hugo/site`.

***NOTE : Assurez-vous de mettre à jour la variable `baseurl` dans le fichier config vers un chemin relatif comme ci-dessous.**

        baseurl = "/"


3. Nous devons mettre à jour notre layout de landing page avec le layout template du thème situé sur `themes/hugo-strata-theme/layouts/index.html`. Pour plus de facilité, voici le code de ce thème (en date du 3 juillet 2017) pour remplacer les contenus de votre fichier `index.html` situés dans `site/layouts/index.html`.

```bash    
        {{ define "main" }}
        {{ if not .Site.Params.about.hide }}
            {{ partial "about" . }}
        {{ end }}

        {{ if not .Site.Params.portfolio.hide }}
            {{ partial "portfolio" . }}
        {{ end }}

        {{ if not .Site.Params.recentposts.hide }}
            {{ partial "recent-posts" . }}
        {{ end }}

        {{ if not .Site.Params.contact.hide }}
            {{ partial "contact" . }}
        {{ end }}
    {{ end }}

```    

4. Pour inclure notre nouvelle page `a-propos` dans cette navigation de thème, ouvrez le fichier `config.toml` que nous avons modifié précédemment et mettez à jour les variables du menu pour inclure la page `a-propos`  comme suit :

```bash

[[menu.main]]
    name = "Accueil"
    url = "/"
    weight = 0

[[menu.main]]
    name = "À Propos"
    url = "a-propos"
    weight = 5

[[menu.main]]
    name = "Blog"
    url = "post/"
    weight = 10

```

Vous verrez maintenant le lien de page `a-propos`  restitué dans la barre latérale de navigation mais si vous cliquez dessus, elle affichera une erreur signalant que la page `a-propos` ne peut pas être trouvée. Ceci parce que nous n’avons jamais modifié le statut de la page de « draft » à « prêt-pour-la-production ». Tout ce que nous devons faire c'est modifier dans le front-matter la valeur de `draft` de `true` vers `false`. Votre fichier `a-propos.md` que nous avons créé précédemment devrait désormais ressembler à quelque chose comme suit :

        +++
    date = "2016-09-09T10:15:23-04:00"
    draft = false
    title = "about"
    +++

    ## Ceci est notre contenu échantillon Markdown.


5. Répétez cette mise à jour de statut draft avec tous les fichiers post que nous avons créés plus tôt (faisant référence au `premier.md` et tous les autres contenus que vous pouvez avoir créés). Ceci vous permettra de voir ces articles listés sur la page d’accueil (dans la section "Recent Blog Posts") et sur la page "Blog" (accédée en cliquant "Blog" dans la navigation latérale).

## **Testez votre Site**

Maintenant que nous avons paramétré le site, jetons un oeil à ce que nous obtenons (si vous n’avez pas déjà fait un suivi en temps réel). Allumez le serveur dans le terminal avec la commande :


    $ npm start


Voilà ! Vous devriez voir votre landing page mise en forme pour ressembler à la photo d’écran ci-dessous :

![capture de la page d'accueil thème Strata](https://monosnap.com/file/0s164EjjfcTk3WxRf5yOGtSC1XTAcQ.png)

Pendant que vous êtes encore effrayé face à  votre nouvelle page de destination, allez-y et consultez également vos nouvelles pages "À propos" et "Blog".

Si tout semble bien fonctionner, il est temps de pousser votre nouveau site hugo vers un repo compte Github (Netlify prend également en charge la liaison vers BitBucket, GitLabs et les repos autonomes).


## **Créez votre Repo Git**

[Créez un nouveau repo sur GitHub](https://help.github.com/articles/create-a-repo/). Pour éviter les erreurs, ne l'initialisez pas avec les fichiers `README`, `license`, ou `gitignore`. Vous pourrez rajouter ces fichiers après avoir poussé votre projet sur GitHub.

Ouvrez une fenêtre de terminal sur votre machine (ou votre outil de ligne de commande de votre choix) et assurez-vous que le répertoire en cours de travail est réglé sur votre projet local. Si non, naviguez vers celui-ci comme ci dessous :

    cd /PATH/TO/hugo

Initialisez le répertoire local sous un repo Git  

    git init

Ajoutez les fichiers dans votre nouveau repo local. Ceci les met en évidence pour le premier commit.

    git add .

Confirmez les fichiers que vous avez placés dans votre dépôt local.

    git commit -m 'Premier commit'

En haut de votre page Quick Setup de votre repo, cliquez sur l'icône presse-papiers pour copier l'URL du dépôt distant.

Dans le terminal, lancez la commande suivante en changeant l'URL pour le dépôt distant où sera poussé votre repository local.  

    git remote add origin VOTRE_URL_REPO_GITHUB

Vérifiez votre URL

    git remote -v

Désormais, il est le temps de pousser les modifications dans votre repo local vers GitHub.    

    git push origin master

À vous de vérifier que votre repo est désormais sur GitHub. Si c'est fait, connectons-le à Netlify.

## Démarrez sur Netlify

Dans cette section, nous vous monterons la facilité avec laquelle vous allez pouvoir lancer votre premier site Hugo sur Netlify. Si vous n'êtres pas déjà utilisateur de Netlify, allez-y et créez tout d'abord un compte gratuit [ici][8].

### Étape 1 : Ajoutez votre Nouveau Site

![étape 1 - ajouter un site git](https://monosnap.com/file/UffmsmsweFWBSgmoj6rILTGXHfwvwa.png)

Créer un nouveau site sur Netlify est simple. Une fois connecté, vous serez amené sur <https://app.netlify.com>. Si vous démarrez pour la première fois, il n'y a qu'une option. Cliquez sur le bouton en haut et à droite **New site from Git** montré au-dessus..

### Étape 2 : Le Relier à Votre GitHub

Cliquez sur **New site from Git**  vous amène à cet écran :

![étape 2 : relier à votre compte Git](https://monosnap.com/file/0TMlE7As2AFRmqvaxnz5fECgfQ3d7J.png)

Si vous poussez sur GitHub, Netlify fait tout le travail. Fini le déploiement manuel de mises à jour ou modifications !

Parce que votre repo est déjà poussé sur GitHub, tout ce que nous devons faire ici, c'est de relier Netlify à GitHub. Cliquez sur le bouton  **GitHub** comme illustré dans la capture d'écran au-dessus.

### Étape 3 : Autoriser Netlify

![étape 3 - autoriser][11]

Il est temps de donner l'autorisation à Netlify et GitHub de pouvoir se parler entre eux. Cliquez sur le bouton **Authorize Application** fera simplement cela. Comme c'est indiqué dans l'image au-dessus, Netlify ne stocke pas votre jeton d'accès GitHub sur ses serveurs. Si vous voulez en savoir plus sur les permissions de Netlify et pourquoi nous en avons besoin, vous pouvez visiter <https://docs.netlify.com/github-permissions/>.

### Étape 4 : Sélectionner votre Repo

![étape 4 - choix du repo](https://monosnap.com/file/rvBTfOYzOes7bEFXcIbl7CfZdBMaE0.png)

Maintenant que vous êtes connecté à Netlify et GitHub, vous pouvez voir une liste de tous vos repos Git. Il y a le repo "hugo" (NDT : chez moi c'est `victor-hugo-strata`) que nous venons de pousser sur GitHub. Sélectionnons-le.

### Étape 5 : Configurer Vos Réglages

![étape 5 - réglages de configuration]

Ici vous pouvez configurer vos options. Assurez-vous dans ce cas que votre répertoire soit  `dist/` et votre commande build `npm run build`. Puis cliquez sur le bouton **Deploy site** en bas de la page pour continuer.

### Étape 6 : Construire Votre Site

![étape 6 - construction][14]

Maintenant il est temps de faire une pause. Vous avez fait votre partie pour laisser Netlify prendre soin du reste - cela ne devrait prendre qu'une minute.

### Étape 7 : Terminé

![step 7 - fait][15]

Netlify a avancé et a donné à votre site un nom temporaire. Mettez-le à jour rapidement afin que ce soit un peu plus joli :



![étape 8 - enjolivons](https://monosnap.com/file/wu3xVizJ0Mk1BRg0eZUOjLsDZoGq7E.png)

Là, c'est mieux. Plus joli non ? Ce n'était pas facile ? Allez un peu plus loin et réglez votre nom de domaine personnalisé. (Apprenez comme faire ça [ici][17]). Bravo, et merci d'utiliser Netlify !



* * *

**About Netlify**

Netlify is an all-in-one platform for deploying and automating modern web projects.

Simply push and Netlify provides everything—servers, CDN, continuous delivery, HTTPS, staging environments, prerendering, asset post processing, DNS, and more.

Any project, big or small, can perform instantly on a global scale.

[1]: https://gohugo.io/
[4]: http://themes.gohugo.io/
[6]: https://raw.githubusercontent.com/digitalcraftsman/hugo-strata-theme/dev/images/screenshot.png
[8]: https://app.netlify.com/signup
[9]: https://cdn.netlify.com/ef2fd8809717bbf8c7a16ac2862c19bb12f717b5/ef6e8/img/blog/add-new-project.png
[10]: https://cdn.netlify.com/6ce8bf46dcc8bfc6d6ef982c7870eb86e32d2b8c/89152/img/blog/step-2-hugo.png
[11]: https://cloud.githubusercontent.com/assets/6520639/9803635/71760370-57d9-11e5-8bdb-850aa176a22c.png
[12]: https://cloud.githubusercontent.com/assets/6520639/9897552/b9ea7f7c-5bfe-11e5-94a0-f957a7d1986e.png
[13]: https://cdn.netlify.com/80976208deb6958b0fd438cb961a63d95a259f07/c1ae2/img/blog/config-your-repo.png
[14]: https://cdn.netlify.com/8f58588b14e3136d2885c3df9875b1b5b8e72f51/0a39b/img/blog/building-site.png
[15]: https://cdn.netlify.com/adbf91d6cdc41c8295b1162e7058b013d52e93f3/aa69f/img/blog/done-1.png
[16]: https://cdn.netlify.com/7af81cfe5062746090a0fd72b6cbea5961f02627/a165c/img/blog/done-2.png
[17]: https://www.netlify.com/blog/2016/03/14/setting-up-your-custom-domain/
