# Surveillance Station

## Présentation

Le plugin Surveillance Station permet de commander Surveillance Station en utilisant l’API officielle de Synology.
Et d’afficher la caméra en direct (live) dans un widget.

![alt text](https://github.com/surveillancestation/surveillancestation/blob/master/docs/images/ss2.png)

## Compatibilité

- DSM 5 et ultérieur
- Surveillance Station version 8.0 minimum (Home Mode à partir de la version 8.1)

## Données visibles sur le Dashboard :

- *Live* : permet d’afficher la caméra en direct (widget redimensionnable avec le crayon, voir FAQ)
- *Activer ou Désactiver* : permet d’activer ou de désactiver une caméra de Surveillance Station (une ou plusieurs caméras)
- *Afficher le statut* : permet d’afficher le statut de la caméra (Désactivée ou Activée)
- *Démarrer et arrêter un enregistrement* : permet de forcer un enregistrement (automatiquement stocker dans Surveillance Station)
- *instantané (Snapshot)* : permet de prendre une capture de la caméra au moment de la demande (automatiquement stocker dans Surveillance Station)
- *Activer ou Désactiver la détection de mouvement* : permet d’activer ou de désactiver la détection de mouvement par scénario (par Surveillance Station ou par Caméra) SEULEMENT quand la caméra est activée
- *PTZ* : permet de contrôler la caméra si celle-ci est compatible. "Patrouille" et "Position prédéfinie" sont aussi disponiblent par scénario
- *Home Mode* : permet d'afficher le statut, d'activer ou de désactiver le Home Mode (mode Accueil) par scénario (commande rattachée à une caméra, mais il s'agit bien d'une activation/désactivation globale)

## Scénario :

Les commandes sont disponibles lors de la création d’un scénario. Voici des exemples d’utilisations :

- Lors de l’activation/désactivation de votre alarme, il est possible d’activer/désactiver automatiquement les caméras de Surveillance Station et le Home Mode (mode Accueil).
- Lors d’une détection d’intrusion, quand l’alarme se déclenche. Il est possible de créer un scénario pour forcer l’enregistrement. Et/Ou de prendre un ou plusieurs instantanés.

## Installation/Configuration
### Installation
Après avoir installé le plugin via le Market. Vous arrivez automatiquement sur cette page :

![alt text](https://github.com/surveillancestation/surveillancestation/blob/master/docs/images/ss1.png)

- Cliquer sur le bouton "Activer"

### Configuration
Nous allons maintenant paramétrer le plugin.

- *Adresse DNS de votre Synolgy* : adresse/host DNS de votre NAS Synology (DSM), et non l'adresse de Surveillance Station (Exemples d'adresses : MonSyno.tld, dsm.chezmoi.fr.). Cette même adresse doit être accessible de l'extérieur (internet) et de votre réseau local (LAN). Dans le cas d'utilisation d'un IP LAN, la seule fonctionnalité qui ne sera pas possible de l'extérieur : visualiser le Live.
- *N° de Port* : N° du port qui est associé à votre adresse (exemple : 443 pour du https)
- *Connexion sécurisée* : à cocher si vous utilisez le https avec un certificat émis et vérifié
- *Identifiant Surveillance Station* : identifiant d'un compte avec les droits : dossier "surveillance" dans "permissions", "Surveillance Station" dans "Applications" et un privilège directeur dans Surveillance Station
- *Mot de passe Surveillance Station* : mot de passe associé à votre identifiant

[IMPORTANT]
Connexion sécurisée doit être utilisée seulement si votre certificat a été émis et vérifié par une Autorité de Certification. Le plugin n'est pas compatible avec le certificat auto-signé par défaut.

Puis, il suffit de se rendre sur la page d'accueil de configuration du plugin, et de cliquer sur : Synchronisation.

Une fois la synchronisation terminée, vos caméras doivent s'afficher dans la zone "Mes caméras de Surveillance Station"

Puis définir pour chaque caméra :

- Objet parent
- Catégorie (optionnelle)
- Activer (Oui, sinon l’équipement ne sera pas utilisable)
- Visible (optionnel si vous ne désirez pas le rendre visible sur le Dashboard. Toutefois, il sera utilisable dans un scénario et visible dans le panel)

## Astuces
### Alerter Jeedom d'une détection provenant Surveillance Station
Il est possible de paramètrer dans Surveillance Station de Synology l'appel d'une url externet (votre Jeedom) en cas d'alerter détection de mouvement. Pour se faire, il faut se rendre dans Surveillance Station, et ajouter une régle d'action.

![alt text](https://github.com/surveillancestation/surveillancestation/blob/master/docs/images/ss10.png)

Puis de renseigner l'url de la commande que vous voulez lancer en cas de détection (exemple pour une commande avec l'id 915 : http://dns_de_votre_jeedom/core/api/jeeApi.php?apikey=b8F......Hb7&type=cmd&id=915

### Alerter Jeedom de l'activation et la désactivation du Home Mode (mode accueil)
Il est possible de demander à Surveillance Station d'alerter Jeedom lors d'un changement de statut de Home Mode, à l'aide du règle, dans Surveillance Station.
Et donc, de faire cohabiter les deux applications :

- d'alerter Jeedom en cas de changement manuel réalisé directement dans Surveillance Station
- d'alerter Jeedom lors de l'activation ou la désactivation de Home Mode par Geofence
- de commander Surveillance Station par Jeedom

Pour commencer, on va récupérer les 2 URL API de Jeedom permettant de lancer les commandes activer/désactiver Home Mode. Il suffit de :

- se rendre dans la configuration d'une de vos caméras
- se rendre sur l'onglet "Commandes"
- cliquer sur l'engrenage pour les 2 commandes "Active Home Mode" et "Désactive Home Mode"
- copier les "URL Direct", et de les coller dans un bloc note par exemple

Nous avons fini côté Jeedom. A noter, que vous pouvez aussi utiliser un virtuel pour récupérer l'information qui proviendra de Surveillance Station

Nous allons maintenant configurer Surveillance Station en ajoutant deux "Règle d'action".

Voici l'explication pour la commande Jeedom "Active Home Mode" (à renouveler pour la désactivation)

- lancer "Règle d'action"
- cliquer sur ajouter
- saisir un nom (par exemple : push activation Home Mode), ne pas modifier le reste, et cliquer sur Suivant
- Source d'évènement, choisir : Surveillance Station
- Évènement : sélectionner "Accéder au mode Accueil", puis cliquer sur Suivant
- Périphérique d'action : choisir "Périphérique externe"
- Url : coller l'URL de la commande Jeedom "Active Home Mode", puis cliquer sur Suivant
- Programmer : personnellement, je laisse Actif partout vu que j'utilise la domotique ou Geofense pour la gestion.

### Recevoir un SMS provenant de DSM/SS en utilisant le plugin SMS de Jeedom
Il est possible de configurer DSM et SS pour lancer des notifications par SMS en utilisant Jeedom avec le plugin SMS.
Il suffit d'ajouter Jeedom comme fournisseur de service SMS.

Avant de commencer la config de DSM, nous allons préparer une URL :

- récupèrons l'URL de la commande correspondante : config plugin SMS, onglet "Commandes", cliquer sur l'engrenage, et copier l'URL directe
- Vous devez mettre de côté les infos suivantes qui sont contenu dans l'URL : apikey et id


- dans DSM (la configuration pourra donc être reprise automatiquement dans SS)
- se rendre dans "Panneau de configuration"
- puis "Notification"
- cocher "Activer les notifications par SMS"
- cliquer sur "Ajouter un fournisseur de service SMS"
- saisir un nom, exemple : Jeedom
- coller l'URL en modifiant seulement l'IP de votre Jeedom : http://10.73.73.100/core/api/jeeApi.php?apikey=&type=cmd&id=&title=Synology&junk=junk&message=Hello+world
- cliquer sur suivant
- Pour apikey= : sélectionner Mot de passe
- Pour type=cmd= : laisser Autre
- Pour id= : sélectionner Nom d'utilisateur
- Pour title= : laisser autre
- Pour junk=junk : sélectionner Numéro de téléphone
- Pour message=Hello+world : sélectionner Contenu du message
- Et cliquer sur Terminer/Appliquer
- Nom utilisateur, saisir l'id de la commande récupéré précédemment (id)
- Mot de passe, saisir la clef API récupérée précédemment (apikey)
- Appliquer les changements, et cliquer sur "Envoyer un message SMS de test"

![alt text](https://github.com/surveillancestation/surveillancestation/blob/master/docs/images/ss7.png)
![alt text](https://github.com/surveillancestation/surveillancestation/blob/master/docs/images/ss8.png)
![alt text](https://github.com/surveillancestation/surveillancestation/blob/master/docs/images/ss9.png)

## FAQ
#### Quelle est la fréquence de rafraichissement des statuts ?
Le plugin actualise les informations toutes les 5 minutes (modifiable dans le "Moteur de tâches")

#### Je ne vois pas mes positions prédéfinies, et mes patrouilles lors de la création d'un scénario :
dès que vous créez une nouvelle position ou patrouille, il faut relancer une synchronisation via le plugin. Permet de remettre à jour la liste dans vos scénarios.

#### J’obtiens une erreur quand je demande l'activation ou la désactivation de la caméra ou un code erreur 117 :
l'identifiant n'a certainement pas les bons privilèges dans Surveillance Station. Modifier le privilège de spectateur à directeur

#### J’obtiens un code erreur 105 :
l'identifiant n'a pas les droits pour utiliser l'application Surveillance Station (panneau de config / Utilisateur / modifier l'utilisateur / onglet Application / cocher Surveillance Station)

#### J’obtiens un code erreur 401 :
l'identifiant est sûrement désactivé dans Surveillance Station. Je vous conseil d'utiliser un identifiant unique avec les droits : dossier "surveillance" dans "permissions", "Surveillance Station" dans "Applications" et un privilège directeur dans Surveillance Station

#### J’obtiens un code erreur 407 :
l'identifiant est bloqué (panneau de config / Sécurité / onglet compte / Autoriser/Bloquer la liste / onglet Liste des blogages)

#### J’obtiens une erreur : Connection refused
Vérifiez bien que l'adresse et le port correspondent bien à votre Synology, et non à Surveillance Station

#### L’affiche du Live déborde du widget (ou trop grand/petit), je désire redimensionner la taille. Comment faire ?
Vous pouvez redimensionner la taille du widget avec le crayon en haut à droite sur le Dashboard.

#### Le redimentionnement du Widget de ma caméra Live ne fonctionne pas. Que faire ?
Le redimensionnement est effectif seulement après actualisation de la page. Pour faciliter le réglage, je vous conseille de choisir une taille du Widget "caméra désactivée". Et de réactiver la caméra, puis d’actualiser à nouveau de Dashboard.

#### En HTTPS, le live de la caméra ne s’affiche pas. Que faire ?
Vous avez certainement un certificat auto-signé (pour le vérifier, dans DSM / Panneau de configuration / Sécurité / certificat). Dans ce cas, le plugin n’est pas compatible (il est toutefois possible d’ajouter une exception dans votre navigateur Internet, mais cette solution risque de ne pas fonctionner sur votre mobile). Je vous conseille de passer par une autorité de certification. Il existe par exemples "StartSSL", "CAcert" et "Let's Encrypt" qui proposent un certificat valide et gratuit (à renouveler une fois de temps en temps suivant l'autorité)

#### L’activation et la désactivation de la caméra ne fonctionnent pas. Que faire ?
Vérifier les privilèges de l’utilisateur dans Surveillance Station (surement que Spectateur, à changer en Directeur).

#### Impossible de désactiver ou d’activer la détection de mouvement. Que faire ?
L’activation ou la désactivation fonctionne seulement quand la caméra est activée. Il faut donc activer la caméra avant de modifier ce paramètre.

## Changelog
[Voir la page dédiée](changelog.md).
