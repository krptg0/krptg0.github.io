+++
categories = ["cloud"]
date = 2017-05-06T14:36:00Z
description = ""
draft = false
slug = "comment-creer-un-serveur-sur-digitalocean"
tags = ["cloud"]
title = "créer un droplet sur DigitalOcean"

+++


**Salut !**

En conséquence directe de mon précédent tutoriel, qui s'appuyait soit sur un VPS soit sur un serveur self hosted, je tenais aujourd'hui à vous montrer simplement comment créer une machine virtuelle chez *DigitalOcean*.

En bref, DigitalOcean est une société d'hébergement de VPS uniquement, qui ne voue loue pas réellement de serveurs physiques, mais plutôt des serveurs virtuels créés à la volée et à la demande (un déploiement/une création de serveur prend moins d'une minute). Ce sont des petites instances clonables et pouvant être détruites à la volée, ce qui permet soit d'être rapide dans le formattage d'une machine, soit de pouvoir sauvegarder et cloner des configurations !

*Alors bien entendu c'est payant, mais si vous êtes étudiant, en vous inscrivant sur [Github Education](https://education.github.com/), vous pouvez obtenir 50$ de crédit pour DO.*

Commencez par vous créer un compte sur [**DigitalOcean**](https://cloud.digitalocean.com/registrations/new), c'est une procédure classique, et connectez-vous.

Une machine virtuelle sur DO s'appelle un ***Droplet***, l'écran sur lequel vous arrivez est le Dashboard qui vous présente vos Droplets actifs.

Pour créer un ***droplet***, cliquez sur le bouton vert éponyme en haut à droite, et c'est ensuite à vous de composer votre ***droplet*** comme bon vous semble.
La force de DO est qu'il propore directement dans un onglet séparé, une pléthore d'application web déjà prête, pouvant répondre le plus souvent aux besoins des utilisateurs avancés. 
Par défaut vous devez choisir une distribution, ce qui vous donnera un accès SSH classique sur une machine vierge.

![](__GHOST_URL__/content/images/2017/05/2017-05-05-16_21_31-.png)

Sur la page principale, vous avez le choix entre pas mal de distributions **Linux**. Chacun sa préférence pour le coup, j'ai tendence à prendre beaucoup de Debian parce que j'y suis habitué, mais chacun sa préférence :)

**DigitalOcean** vous facturera à l'heure, à partir de la première minute, c'est-à-dire que si vous comptez utiliser un ***droplet*** pour une utilisation ponctuelle, pensez bien à l'éteindre quand vous finissez votre travail pour ne pas dépenser votre argent inutilement !

![](__GHOST_URL__/content/images/2017/05/2017-05-05-16_25_49-DigitalOcean---Create-Droplets.png)

Vous avez également la possibilité d'ajouter du stockage sur votre machine (préférez Amazon pour le stockage cloud), et la **localisation**.

*Ce dernier choix est relativement important car il impactera directement sur, d'une part, la latence de connexion, choisir un endroit proche de vous sera logiquement plus rapide, et de deux en termes de légalité, vous dépendez de la législation du pays où est installé votre droplet au cas où vous décidiez de faire des choses pas super légales ! Ca reste bon à savoir :)*

Pour une utilisation classique, vous n'avez pas besoin de vous attarder sur les options suivantes:

![](__GHOST_URL__/content/images/2017/05/2017_05_05_16_29_28_DigitalOcean_Create_Droplets.png)

Choisissez un nom sympa pour votre VM et go !

Vous recevrez un mail comportant l'IP de votre machine, et le mot de passe root par défaut pour celle-ci.

Téléchargez un logiciel comme [PuTTY](https://the.earth.li/~sgtatham/putty/latest/w64/putty-64bit-0.69-installer.msi) qui vous permettra d'établir une connexion **SSH** rapidement. A la première connexion vous serez invité à changer le mot de passe root par défaut, et vous êtes good to go !

Notez que je suis actuellement sous Windows 10, mais avec un logiciel qui se nomme Babun, packagé avec Cygwin, permettant d'avoir un système UNIX-like sur Windows, ça permet de garder les meilleurs réflèxes, j'en ferai un article d'ailleurs !

