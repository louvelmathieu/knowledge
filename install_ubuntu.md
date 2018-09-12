# Configuration Ubuntu

## PHP

### Installation de php7.1
> https://www.noobunbox.net/serveur/auto-hebergement/installer-php-7-1-sous-debian-et-ubuntu

On ajoute un repo pour avoir la version 7.1

```
sudo apt-get install -y python-software-properties
sudo add-apt-repository -y ppa:ondrej/php
sudo apt-get update -y
sudo apt-get install php7.1-fpm 
```

### Installation des extensions

```
sudo apt-get install php7.1-mysql php7.1-curl php7.1-json php7.1-gd php7.1-mcrypt php7.1-msgpack php7.1-memcached php7.1-intl php7.1-sqlite3 php7.1-gmp php7.1-geoip php7.1-mbstring php7.1-redis php7.1-xml php7.1-zip php7.1-soap php7.1-imagick
```

### Configuration php avec votre utilisateur
> https://unix.stackexchange.com/questions/30190/how-do-i-set-the-user-of-php-fpm-to-be-php-user-instead-of-www-data

```
sudo gedit /etc/php/7.1/fpm/pool.d/www.conf
```

Remplacer `user = www-data` par `user = VOTRE_UTILISATEUR` et redemarrer le service :

```
sudo /etc/init.d/php7.0-fpm restart
```

## Composer
> http://www.ifdattic.com/how-to-use-composer/

```
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

## Nginx

```
sudo apt-get install nginx
```

Si vous utiliser un dossier specifique pour vos vhost, rajouter le dans `/etc/nginx/nginx.conf` : `include my_path/*;`
Les vhosts de developpement sont versionné dans les git, vous pouvez donc tous les ajouter dans votre conf.

ex :

```
include /home/mathieu/htdocs/*/vhost;
```


Redemarrer le service :

```
sudo service nginx restart
```



## NodeJS
> https://doc.ubuntu-fr.org/nodejs , 
> http://nodesource.com/blog/installing-node-js-8-tutorial-linux-via-package-manager/

La documentation officiel install une ancienne version :
```
sudo apt-get install nodejs npm nodejs-legacy
```

Pour installer une version plus recente utiliser :

```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```

## GIT

```
sudo apt-get install git
```

```
git config --global user.name NAME 
git config --global user.email EMAIL
```

### Alias 

Editer le fichier `~/.gitconfig` pour ajouter vos alias.

Ex :
```
[alias]
    st = status
    co = checkout
    br = branch
```

### Cle SSH

```
ssh-keygen -t rsa
```

Copier la cle publique `~/.ssh/id_rsa.pub` dans votre inteface Git(Lab|Hub|...)


### OH MY ZSH

Installation ZSH
```
sudo apt install zsh

```

Installtion oh my zsh

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

Parametrage shell par defaut
```
 chsh -s /bin/zsh 
(Besoin de fermer ou re-ouvrir la session pour avoir le changement)
```

Installer le theme de votre choix  : https://github.com/robbyrussell/oh-my-zsh/wiki/themes

```
sudo gedit ~/.zshrc
```

Modifier le fichier avec par exemple le theme `amuse`

```
ZSH_THEME=amuse
```

Changement dans phpStorm, ouvrez le menu Settings->Tools->Terminal et modifiez Shell Path = /bin/zsh


### PhpStorm

Installation PhpStorm
```
sudo snap install phpstorm --classic
```

Installer les plugins suivant :
* Makefile
* SensiolabsInsight
* PHP Annotations


### Mailtrap

Pour rediriger tous email envoyé via vos scrit sur mailtrap, on utilise ssmtp:

```
sudo apt-get install ssmtp
```

Editer le fichier de configuration
```
sudo gedit /etc/ssmtp/ssmtp.conf
```
 avec ce fichier (en modifiant l'adresse d'expediteur par votre adresse)
 
 ```
#
# Config file for sSMTP sendmail
#
# The person who gets all mail for userids < 1000
# Make this empty to disable rewriting.
root=mathieu.louvel@mindid.fr

# The place where the mail goes. The actual machine name is required no 
# MX records are consulted. Commonly mailhosts are named mail.domain.com
mailhub=smtp.mailtrap.io

# Where will the mail seem to come from?
rewriteDomain=mindid.fr

# The full hostname
hostname=mindid.fr

AuthUser=fec61c73e547fb
AuthPass=1282451cc37cf4

# Are users allowed to set their own From: address?
# YES - Allow the user to specify their own From: address
# NO - Use the system generated From: address
FromLineOverride=YES
```

### Config DNS

Pour rediriger automatiquement toute les adresses en .localhost vers 127.0.0.1 sans passer par le fichier vhost vous pouvez utiliser les masques DNS (ne fonctionne pas avec chrome)

```
sudo gedit /etc/dnsmasq.conf
```

Sous la ligne `# address=/double-click.net/127.0.0.1`, rajouter :

```
address=/localhost/127.0.0.1
```

Redemarrer le service :
```
sudo /etc/init.d/dnsmasq restart
```
