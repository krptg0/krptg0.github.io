+++
categories = ["ssh", "vpn", "hardware"]
date = 2017-11-27T13:09:11Z
description = ""
draft = true
slug = "tunnel-ssh-et-vpn-ou-comment-e-r-les-firewalls-la-pratique"
tags = ["ssh", "vpn", "hardware"]
title = "tunnel SSH et VPN (ou comment e*****r les firewalls), la pratique"

+++


*Ca va devenir un poil plus technique ici, mais pour un passionné d'informatique, tout devrait relativement bien se passer, bon courage ! ;)*
##Prérequis
Ce tutoriel va s'appuyer sur 3 logiciels Windows, assumant que la majorité des utilisateurs concernés par ce tutoriel utilisent Windows, ce sera plus simple. Par contre, côté serveur, il s'agira d'une distribution Debian la plus classique possible, étant donné que les outils SSH de base sont déjà installé, il n'y aura pas besoin de configuration autre que le changement du port d'écoute et de la configuration du serveur VPN (clé en main avec un script tout prêt)

###Serveur
- **VPS chez un hébergeur**: pour 3 à 5€ par mois vous disposez d'une machine qui paraît ridicule niveau performances, mais qui suffit largement pour ce que vous voulez en faire. Je reviens à l'introduction à ce niveau là, lorsque je disais que le coût financier de ce tutoriel s’approchait du coût d'un VPN commercial, voilà pourquoi. Seulement, sur ce VPS vous pourrez installer le serveur SSH mais également vous en servir comme machine de développement/bac-à-sable pour vos projets futurs (seedbox, plex...), d'où l'avantage du DIY ;)
- **OU serveur auto-hébergé**: on s'affranchit du VPS pour héberger chez soi le serveur SSH. Cela à pour avantage, à condition d'avoir le matériel à disposition, de ne pas débourser un sou pour le serveur, mais dont le coût sera répercuté sur, pas exemple, votre facture d'électricité. La sécurité y passe également car vous serez identifié en sortie de réseau par votre box, donc il sera facile de remonter jusqu'à vous quoi que vous fassiez avec cette connexion sécurisée.

*Peu importe le choix, l'installation reste la même, la procédure reste la même, la seule différence est que dans le cas d'un serveur auto-hébergé, il faudra également configurer la redirection de port sur votre box.*

***Veuillez noter qu'en étant étudiant et disposant d'une adresse mail propre à votre établissement d'enseignement, vous pouvez bénéficier du pack [Github éducation](https://education.github.com/), dans lequel vous trouverez un bon de 50€ de crédit chez [DigitalOcean](https://www.digitalocean.com/), qui permettra de faire tourner un VPS (appelé Droplet), pour environ 10 mois, de quoi vous faire une idée avant de mettre la main au porte-feuille :)***

###Client
- **Bitvise SSH Tunnelier**: : logiciel gratuit pour une utilisation privée, c'est celui qui ouvrira le tunnel entre votre machine et votre serveur SSH. Lorsqu'on le ferme, le tunnel est fermé et plus rien ne transite de manière sécurisée.
- **Proxifier**: logiciel shareware avec période d'essai, c'est lui qui fera passer toutes les **connexions** de votre machine par le tunnel SSH.
- **OpenVPN GUI**: Permettra la connexion au serveur VPN.

*Relativement simple, l'installation du client nécessite deux logiciels, assez facile d'utilisation, pas gourmands en ressources et royalement efficaces !*

## Installation
###Serveur
La première étape consiste à changer le port d'écoute par défaut (22) du serveur **SSH** par un port ouvert sur tous les réseaux (443)
####Service SSH
On commence donc par la configuration du serveur:

- **Pour le self-host**: L'installation de debian étant plutôt simple, je vous mets le lien de l'iso le plus rapide à récupérer, à vous ensuite de l'installer comme vous le souhaitez, via une clé USB  sur un ordinateur dédié, ou en machine virtuelle, comme bon vous semble. [Lien vers l'ISO Debian 8 NetInstall](http://saimei.acc.umu.se/debian-cd/8.6.0/amd64/iso-cd/debian-8.6.0-amd64-netinst.iso)

- **Pour le VPS**: Veillez bien lors de l'achat du VPS à choisir **debian** comme distribution à installer. Notez également l'IP de votre serveur, la méthode de connexion préférée par votre provider (login/mdp SSH ou par clé), ainsi que la version de Debian (si possible préférez une Debian 8)

Connectez vous en **SSH (root)** sur votre serveur, depuis chez vous, ou tout endroit où le port **22** n'est pas bloqué. Vous devriez alors avoir un prompt sur votre serveur.

    root@debian~#

La première chose à faire est de créer un utilisateur différent du root, qui n'aura accès qu'au tunnel SSH, par soucis de sécurité, il ne faudrait pas que tout le monde puisse foutre le bordel sur votre cher serveur :)

    root@debian:~# useradd toutlemonde -d /dev/null -s /bin/rbash
    root@debian:~# passwd toutlemonde

L'option *-d* indique que la répertoire *home* de l'utilisateur toutlemonde n'existera pas théoriquement, l'option *-s* indique le shell par défaut de l'utilisateur, ici **rbash** (pour restricted bash), qui n'autorise que la commande **ssh**, cela évitera les mauvaises manipulation si un terminal est ouvert par le client.

Voilà notre utilisateur est créé, et il à la possibilité de se connecter en **SSH** au serveur, vous pouvez tester !

    lgromb@mymachine: ssh toutlemonde@IP.SERVEUR
    toutlemonde@debian:

La seconde étape est de faire écouter le serveur **SSH** sur le port 443, pour cela il faut éditer le fichier /etc/ssh/sshd_config (en étant connecté en tant que root):

    nano /etc/ssh/sshd_config

Trouvez la ligne 

    Port 22

Et editez-là en 

    Port 443

Enregistrez le fichier avec **Ctrl+X**, confirmez avec O ou Y (selon la langue de votre système), et redémarrez le service ssh

    service ssh restart

La partie SSH est terminée côté serveur, vous pouvez d'ores et déjà essayer de vous connecter de la même manière que plus haut, avec un petit changement:

    lgromb@mymachine: ssh -p 443 toutlemonde@IP.SERVEUR

####Service VPN
Le but étant d'ouvrir au maximum les portes fermées, nous allons connecter par dessus la connexion SSH une connexion VPN. L'idéal étant de faire cohabiter le serveur SSH et le serveur VPN sur le même serveur pour réduire les coûts.

Plein d'articles sur Internet *(même Wikipedia)* vous permettrons d'en apprendre plus sur le **VPN**. Vous devez retenir que ce protocole permet de relier deux réseaux entre eux, comme s'il étaient physiquement liés par un switch ou un hub. En gros imaginez un réseau d'entreprise ultra sécurisé, et vous êtes chez vous. Vous avez besoin d'accéder à vos documents qui sont sur les serveurs de cette entreprise, pour cela vous utiliserez les outils **VPN** mis à votre disposition par cette entreprise. Ca permettra à votre ordinateur d'être physiquement vu par le réseau de l'entreprise comme DANS l'entreprise alors qu'en réalité vous êtes tranquillement chez vous. *Security breaches much ?* Probablement, si un mauvais usage en est fait, ce n'est pas à moi de juger dans le cas d'utilisation en entreprise. Par contre pour un usage comme indique ici, c'est tout à fait approprié !

Ici on va se baser sur **OpenVPN**, et plus précisement sur OpenVPN Access Server, qui est une solution clé en main pour obtenir un serveur **VPN** rapidement prêt à l'emploi. On verra quelques tricks permettant notemment d'obtenir un fichier *.ovpn* plus facile à manipuler en lieu et place d'un fichier *.msi* que le serveur force à télécharger s'il détecte la machine sous Windows. Merci à Korben grace à qui j'ai (re)trouvé l'idée d'OVPNAS :D

#####Prérequis
Pour mener à bien la suite de ce tutoriel, je vous rappelle que vous devez disposer d'un VPS sous Debian 8, ou un serveur self-hosted, avec un serveur **SSH** écoutant sur le port **443** et un utilisateur *toutlemonde*.
#####Installation
Connectez-vous en root sur votre serveur. Lancez une mise à jour du système et l'installation d'openvpn au passage:

    apt update && apt upgrade -y && apt install openvpn -y

Récupérez et installez le package OpenVPN Access Server [sur cette page](http://swupdate.openvpn.org/as/openvpn-as-2.1.4-Debian8.amd_64.deb). Exemplace avec la version 2.1.4:

    wget http://swupdate.openvpn.org/as/openvpn-as-2.1.4-Debian8.amd_64.deb
    dpkg -i openvpn-as-2.1.4-Debian8.amd_64.deb
    passwd openvpn

Renseignez le mot de passe pour le compte d'administration *openvpn*

A la fin de l'installation, il est nécessaire de reconfigurer l'installation par défaut pour débloquer certaines portes.

    root@vps:~# /usr/local/openvpn_as/bin/ovpn-init 

Répondez **DELETE** au premier invite puis **yes** pour accepter la licence.

    Will this be the primary Access Server node? 
    yes

    Please specify the network interface and IP address to be used by the Admin Web UI
    (1) all interfaces

    Please specify the port number for the Admin Web UI. 
    943
   
    Please specify the TCP port number for the OpenVPN Daemon.
    2211

Répondez en appuyant sur Entrée pour le reste des questions (choix par défaut).
Notez également l'adresse d'administration du serveur OpenVPN. C'est simplement l'adresse IP de votre serveur, en https, sur le port 943. N'oubliez pas */admin* à la fin de l'URL. Rendez-vous immédiatement sur cette interface avec votre navigateur (pas besoin d'être connecté au tunnel SSH).

Identifiez-vous avec le login ***openvpn*** et le mot de passe rentré à la première étape.
Vous pourrez ici peaufiner votre serveur, mais ce qui nous intéresse c'est le menu **User Permissions**. Dans ce menu, ajoutez l'utilisateur ***toutlemonde*** et cliquez sur **Save Settings** puis sur **Update Running Server**. Vous pouvez fermer la fenêtre et revenir sur votre terminal.

    cd /usr/local/openvpn_as/scripts
    ./sacli --user toutlemonde GetUserlogin >mon-serveur.ovpn

Le fichier de configuration client est maintenant créé, il s'agira de le récupérer sur votre machine cliente via, par exemple, scp comme cela:

    scp -P 443 root@mon-serveur:/usr/local/openvpn_as/scripts .

Vous pouvez utiliser **FileZilla** ou **WinSCP** pour vous faciliter la tâche.
Le serveur VPN est maintenant opérationnel, le tunnel SSH également, il est de temps de passer côté client !

###Client

Voilà la partie la plus "simple" du tutoriel, c'est beaucoup de *suivant* et un peu de *terminé* ! :D

Vous aurez besoin des 3 logiciels que j'ai cité plus haut:

- [**BitVise**](https://bvdl.s3-eu-west-1.amazonaws.com/BvSshClient-Inst.exe)
- [**Proxifier**](https://www.proxifier.com/distr/ProxifierSetup.exe)
- [**OpenVPN**](https://swupdate.openvpn.org/community/releases/openvpn-install-2.4.1-I601.exe)
    
####Bitvise
On commence par Bitvise, installez le comme un logiciel classique, et à la fin de l'installation une fenêtre s'ouvrira. Remplissez les différents champs et onglets comme suit:

######Onglet *Login* :
- **Host** : Serveur de destination, adresse IP ou hostname
- **Port** : 443
- **Username** : toutlemonde
- **Initial method** : password
- **Password** : ... le mot de passe !
- **Cochez** "Store encrypted password in profile"

######Onglet *Options* :

- **Décochez** "Open Terminal"
- **Décochez** "Open SFTP"
- **Décochez** "show authentifcation banner automatically

######Onglet *Services* :
- **Cochez** *"Enabled"* dans la colonne de droite : **SOCKS/HTTP Proxy Forwarding**
- **Listen Port** : 1234

Tentez de vous connecter, Accept and Save à l'invite pour l'ID du serveur, et enregistrez le profil dans un dossier que vous retrouverez, ce fichier de profil contiendra toutes les informations nécessaire à la connexion à votre serveur.

Ces indications sont personnelles, vous êtes libres de modifier la configuration du logiciel pour vous satisfaire pleinement !

La connexion est opérationnelle lorsque cette dernière ligne est affichée dans la fenêtre de log :

    Enabled SOCKS/HTTP proxy forwarding on 127.0.0.1:1234

Le tunnel **SSH** est maintenant ouvert, n'attendant plus que les connexions de votre ordinateur ;). Pour se faire on va utiliser Proxifier.

####Proxifier

Lancez le setup et installez comme n'importe quel logiciel. A l'issue de l'installation, lancez le et acceptez l'utilisation gratuite pendant 30 jours. La fenêtre de log s'affiche. 
Commencez par cliquez sur **File -> New Profile** puis **File -> Save Profile As** et enregistrez le au même endroit que votre profil Bitvise pour plus de clarté.

Cliquez ensuite sur la première icône de la barre d'actions:
![proxifier1](__GHOST_URL__/content/images/2017/05/proxifier1.png)

Cette fenêtre va s'ouvrir:
![proxifier2](__GHOST_URL__/content/images/2017/05/proxifier2.png)

Cliquez sur **Add** et renseignez les champs comme suit:

- **Address** : 127.0.0.1
- **Port** : 1234
- **SOCKS Version 5**

Cliquez sur **Check** et si tout fonctionne, vous devriez obtenir cette fenêtre :
![proxifier4](__GHOST_URL__/content/images/2017/05/proxifier4.png)

Une fois de plus félicitations ! Les connexions de votre ordinateur passeront dorénavant par le tunnel SSH ! Il reste cependant encore quelques petites choses à voir.

Cliquez sur **OK** pour fermer la fenêtre et répondez **Oui** à l'invite qui se présente:
![proxifier5](__GHOST_URL__/content/images/2017/05/proxifier5.png)

Cliquez sur **OK** pour valider la validation (oui je sais), puis encore une fois sur **OK** pour fermer la fenêtre des serveurs proxys.


A cet instant, **Proxifier** est configuré pour forwarder les connexion des applications *tierces* mais pas les applications systèmes (comme **OpenVPN** par exemple ;) ). Si vous essayez également de joindre un ordinateur sur votre réseau local, cela passera également par le tunnel, il faut donc imposer des règles de redirections à **Proxifier** pour être sur qu'il ne s'occupe que de ce qu'il faut.

En cliquant sur la deuxième icône de la barre d'outils, vous arriverez sur les règles de **Proxifier**, comprendre *"comment dois-je diriger quelle connexion par où ?"* Par défaut, vous observez que les connexions locales de l'ordinateur, la boucle locale et ce qui fait référence à l'ordinateur lui même n'est pas redirigé dans le tunnel **"Action: Direct"**. Par contre, pour le reste des connexions, elles seront toutes redirigées par le tunnel **"Action: Proxy SOCKS5 127.0.0.1"**. A vous de jouer un peu avec ces règles pour obtenir votre configuration idéale :). A savoir également, les règles s'appliquent par ordre de classement : La première avant la deuxième, etc..

Je disais que **Proxifier** redirigeait les connexions des applications tierces. Pour être sûr de bien être sécurisé et de couvrir l'ensemble des logiciels de l'ordinateur, il faut modifier un paramètre afin de rediriger également les connexions système, il est ici :

![proxifier6](__GHOST_URL__/content/images/2017/05/proxifier6.png)

Cochez la deuxième case et on est bons !

![proxifier7](__GHOST_URL__/content/images/2017/05/proxifier7.png)

Cliquez sur **File -> Export Profile** et enregistrez le dans votre dossier de sauvegarde, que vous pouvez maintenant compresser et uploader où vous voulez :) Faites attentions à qui vous le partagez cela dit, je conseillerai de mettre un mot de passe sur l'archive...

####OpenVPN

Installez OpenVPN comme d'habitude. Placez le fichier .ovpn récupéré précedemment dans le dossier **C:\Program Files\OpenVPN\config** à condition que vous n'ayez pas changé le répertoire d'installation par défaut.

Lorsque vous lancerez **OpenVPN GUI**, une icône apparaîtra dans la zone de notifications, cliquez droit dessus et cliquez sur Connect en rentrant le login *toutlemonde* ainsi que son mot de passe.

Une fois la connexion établie, via le tunnel, vous aurez absolument tout terminé ! Votre connexion devrait être stable dans la mesure du possible, n'oubliez pas que vous avez une connexion **VPN** encapsulée dans un tunnel SSH, ce n'est pas conventionnel et ça peut engendrer quelques sauts de connexions de temps à autres. 

#Conclusion

J'espère que ce tutoriel aura été simple à suivre et efficace pour beaucoup d'entre vous, j'ai pas mal blablaté, expliqué autant de choses que possible sans essayer de trop en faire et je sais pertinament que pour qui viendrait chercher uniquement de la technique, il y a trop de texte. Je ré-adapterai peut être ce tutoriel sous forme purement technique pour aller droit au but, à voir :)

En tout cas si ça vous a servi, n'hésitez pas à partager cette méthode avec votre entourage, ça peut débloquer pas mal de situations même si j'ai conscience que le niveau de productivité où que vous soyez peut être drastiquement réduit haha.

Vous trouverez mes réseaux sociaux et mon mail en page d'accueil de ce blog où vous pourrez me poser toutes vos questions si vous avez besoin d'aide !

Merci et à bientôt :)

