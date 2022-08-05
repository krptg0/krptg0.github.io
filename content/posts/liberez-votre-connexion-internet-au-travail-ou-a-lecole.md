+++
categories = ["software", "ssh", "vpn", "hardware"]
date = 2017-05-03T08:38:14Z
description = ""
draft = true
image = "__GHOST_URL__/content/images/2017/05/maxresdefault.jpg"
slug = "liberez-votre-connexion-internet-au-travail-ou-a-lecole"
tags = ["software", "ssh", "vpn", "hardware"]
title = "tunnel SSH et VPN (ou comment e*****r les firewalls), le blabla"

+++


**Après** y avoir longuement réfléchi, je me lance dans la rédaction de ce tutoriel qui permettra à tout un chacun de pouvoir disposer d'une connexion sécurisée, débridée et illimitée quel que soit le lieu de connexion (travail, école, réseau Wi-Fi public...).

**Cela** nécessitera un peu de travail, mais une fois l'ensemble en route et en production, la maintenance n'est que minime, et le coût relativement accessible au vu des fonctionnalités :)  

#Le contexte

Comme vous devez le savoir, pour éviter que Jean-Kevin puisse jouer à **Dofus** tout en étant en cours de physique dans son lycée, à compter qu'il ait à disposition un ordinateur, les établissement bloquent généralement tout ce qui n'est pas trafic Internet. Lorsque vous consultez une page web, vous envoyez une requête à un serveur sur (généralement, très très généralement) le port *80* pour une page **HTTP**, ou le port *443* pour une page sécurisée **HTTPS**. 
Pour éviter de bloquer absolument **99,9%** d'Internet, ce sont donc les deux seuls ports ouverts dans ces établissements. Si l'on compte que Dofus communique sur le port **5555**, et qu'il n'est pas ouvert, alors vous ne pourrez pas vous y connecter. Solution simple et radicalement efficace pour empêcher les distractions sur ces lieux de travail.

La technique habituellement utilisée pour contourner cela est d'utiliser un **VPN**, comme *FrozenWay*, qui permet de faire passer l'ensemble des connexions de l'ordinateur client dans un tunnel sécurisé, communiquant avec son serveur sur un des deux ports ouverts, **80** ou **443**. Ces entreprises fournisseurs de VPN sont souvent gratuites à débit limité, et demandent aux utilisateurs d'allonger un peu de monnaie par mois pour débloquer l'ensemble des fonctionnalités. Alors certes, le coût de revient de ce tutoriel revient sensiblement au coût d'un tel service, mais vous n'apprendrez rien et pas moyen de vous la péter en disant "je l'ai fait tout seul :D".

L'inconvénient de l'usage direct d'un VPN est que bien souvent les réseaux d'entreprise sont configurés pour bloquer le traffic des VPN par analyse des trames des paquets, reconnaissant de suite le protocole VPN, et ce peu importe le port de connexion.

La technique sur laquelle va se baser ce tutoriel est la technologie SSH. C'est un protocole de communication sécurisé et basique, disponible généralement pour administrer à distance des serveurs sous *NIX (par exemple). SSH dispose en effet d'une pléthore d'options permettant d'en faire à peu près ce qu'on veut, y compris servir de passerelle à un utilisateur, comme un proxy !

La pratique est simple, il s'agit, sur l'ordinateur client, de lancer une connexion **SSH** vers un serveur **SSH** écoutant sur un des deux ports ouverts de l'établissement (**80** ou **443** pour rappel), et ensuite de faire transiter toutes les connexions de l'ordinateur client via ce tunnel ouvert. 
Revenons à **Dofus** : une fois le tunnel ouvert et le réseau de l'ordinateur configuré pour passer intégralement dans ce tunnel, Dofus n'y verra que du feu et initiera sa connexion sur le port 5555 via ce tunnel, qui apparaîtra au réseau de l'entreprise comme une connexion **"web"** car communiquant sur le port **80** ou **443**. Comme Dofus, n'importe quelle application non-web sera à même de communiquer, de se connecter, donc d'être "débloquée" en utilisant ça.

Il existe quelques limitations au niveau des applications complexes utilisant une rangée de ports plus large, comme *Steam*, qui nécessite d'ouvrir une connexion VPN par dessus le tunnel **SSH**. Arrivé à ce point, plus aucun proxy ou firewall ne vous résistera ;)

Voici un petit schéma qui explique relativement simplement le fonctionnement d'un tunnel **SSH**:

![http://technologyordie.com/wp-content/uploads/2012/07/ssh_tunnel.jpg](__GHOST_URL__/content/images/2017/05/sshtunnel.jpg)

*Comme vous pouvez le voir, le tuyau bleu qui représente le tunnel perce complètement le firewall, pour atterrir directement de l'autre côté sur son serveur SSH de destination, ainsi tout le trafic du client passe dans ce tunnel et bypasse les restrictions.*

Je créerai un autre post pour la partie technique

