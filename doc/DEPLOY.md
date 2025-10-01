# Procédure de Déploiement

Décrivez ci-dessous votre procédure de déploiement en détaillant chacune des étapes. De la préparation du VPS à la méthodologie de déploiement continu.

## Préparation du VPS

### Connexion SSH
Je me connecte au serveur avec :
```bash
ssh root@172.17.4.8
```

### Installation d'aaPanel 
J'installe aaPanel avec la commande officielle :
```bash
URL=https://www.aapanel.com/script/install_7.0_en.sh && if [ -f /usr/bin/curl ];then curl -ksSO "$URL" ;else wget --no-check-certificate -O install_7.0_en.sh "$URL";fi;bash install_7.0_en.sh aapanel
```

Je note les identifiants d'accès affichés à la fin de l'installation :

**username: aczyl2pu**

**password: fdfb3aab**

### Configuration aaPanel
1. Je me connecte sur l'interface web d'aaPanel
2. J'installe la stack LNMP (Linux + Nginx + MySQL + PHP)
3. Je crée un site web avec l'IP du serveur (172.17.4.8)
4. Dans les paramètres du site, je modifie le répertoire racine vers `/public`
5. Je crée une base de données depuis l'interface Databases

### Configuration base de données
Dans aaPanel > Databases :
- Nom BDD : `habit_tracker`
- Utilisateur : `habit_tracker`  
- Mot de passe : p7fB3WdrkEAEKpti

Je crée le fichier `.env` dans `/www/wwwroot/172.17.4.8/` :
```env
DB_HOST=localhost
DB_PORT=3306
DB_DATABASE=habit_tracker
DB_USERNAME=habit_tracker
DB_PASSWORD=p7fB3WdrkEAEKpti
```

### Configuration nginx rewrite
Dans aaPanel > Website > 172.17.4.8 > URL rewrite, je configure :
```nginx
location / {
    try_files $uri /index.php?$args;
}

location ~ \.php$ {
    fastcgi_pass unix:/tmp/php-cgi-82.sock;
    fastcgi_index index.php;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    include fastcgi_params;
}
```

### Création des tables de base de données
Je me connecte au serveur et j'exécute :
```bash
cd /www/wwwroot/172.17.4.8
php bin/create-database
```
Cette commande crée automatiquement toutes les tables et les données de démonstration.

## Méthode de déploiement

### Dépôt Git bare
Sur le serveur, je crée un dépôt Git bare :
```bash
mkdir -p /var/depot_git
cd /var/depot_git
git init --bare
```

En local, j'ajoute le remote :
```bash
git remote add vps root@172.17.4.8:/var/depot_git
```

### Script de déploiement
Je crée `deploy.sh` sur le serveur :
```bash
#!/bin/bash
echo "Déploiement de la version $1..."
git --work-tree=/www/wwwroot/172.17.4.8 --git-dir=/var/depot_git checkout -f $1
echo "Code déployé avec succès"
```

Je rends le script exécutable :
```bash
chmod +x deploy.sh
```

### Workflow de déploiement
1. Je commite les changements en local
2. Je pushe sur le VPS : `git push vps main`
3. Sur le serveur : `./deploy.sh main`
4. Si première installation : `php bin/create-database`
5. Je vérifie le bon fonctionnement
