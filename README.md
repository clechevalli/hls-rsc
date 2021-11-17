## **Procédure d’installation pour la démo HLS :**

**Installer un serveur Nginx-Php :**
Exécuter les commandes suivantes :

    sudo -s
    apt install nginx php7.4-fpm php7.4 npm
    service nginx stop
    service php7.4-fpm stop

**Installer hls.js :**
Exécuter les commandes suivantes :

    exit
    cd /var/www/html/
    git clone [https://github.com/video-dev/hls.js.git](https://github.com/video-dev/hls.js.git)
    cd hls.js
    npm install ci
    npm run dev

Effectuer Ctrl+c pour arrêter *npm*
**Récupérer le dépôt *hls-rsc* et mettre en place les scripts d'encodage :**

    cd /var/www/html
    git clone https://github.com/video-dev/hls.js.git
    mkdir live
    mkdir video
    cp hls-rsc/create_vod.sh .
    cp hls-rsc/create_live.sh .
**Appliquer la nouvelle configuration :**

    sudo -s
    rm /etc/nginx/sites-available/default
    cd hls-rsc/
    cp default /etc/nginx/sites-available/
    cp videos /etc/nginx/sites-available/
    cp live /etc/nginx/sites-available/
    ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
    ln -s /etc/nginx/sites-available/videos /etc/nginx/sites-enabled/
    ln -s /etc/nginx/sites-available/live /etc/nginx/sites-enabled/
    cp hls-demo.js /var/www/html/hls.js/dist/
    cp index.php /var/www/html/hls.js/demo/
    cd ..
    chmod a+x live
    chmod a+x video
   **Démarrer les services :**
   

    service nginx start
    service php7.4-fpm start
**Ajout des hosts :**
Sur Windows, il faut ajouter cette ligne dans le fichier *C:\Windows\System32\drivers\etc\hosts* :

    localhost hlstest.com videos.com live.com
  Sur Linux, il faut ajouter cette ligne dans le fichier */etc/hosts* :

    localhost hlstest.com videos.com live.com

## Tester la démo :
Pour démarrer la démonstration, il faut trouver une vidéo assez longue (7-8min) et la mettre dans le dossier */var/www/html/*. Ensuite, il faut exécuter la commande :

    /var/www/html/create_vod.sh
Cette commande permet de créer les différentes qualité de la vidéo de test et de créer les fichiers essentiels à HLS. Il faut ensuite attendre l'encodage complet de la vidéo.
Pour tester le streaming VOD, il faut ouvrir un navigateur et se rendre sur la page hlstest.com/demo/. Vérifier que **VOD Test** est bien sélectionné dans la liste déroulante.
Pour tester le streaming live, il faut cliquer sur le bouton en bas de page **Activer live** et sélectionner **Live Test** dans la liste déroulante.
