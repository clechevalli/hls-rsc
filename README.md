## **Installation  procedure to recreate HLS demo**

**Install Nginx-Php server**

    sudo -s
    apt install nginx php7.4-fpm php7.4 npm   
    service nginx stop
    service php7.4-fpm stop
    exit

**Install hls.js**

Clone the repository :

    cd /var/www/html/
    git clone https://github.com/video-dev/hls.js.git
    
Run hls.js wih npm in order to create usefull files

    cd hls.js
    npm install ci
    npm run dev

Stop npm with Ctrl-C

**Clone the repository *hls-rsc* and set encoding scripts**

    cd /var/www/html
    git clone https://github.com/clechevalli/hls-rsc.git
    mkdir live
    mkdir video
    cp hls-rsc/create_vod.sh .
    cp hls-rsc/create_live.sh .

create_vod.sh and create_live.sh are shell scripts wich use Ffmpeg to encript and fragment a mp4 video into m3u8, in different quality, for Live and Vod.

**Apply nginx configuration**

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
    cp style.css /var/www/html/hls.js/demo/
    cd ..
    chmod a+x live
    chmod a+x video
    
   **Run services**
   
    service nginx start
    service php7.4-fpm start
    
**Add hosts**
On Windows, you have to add this line in the file : *C:\Windows\System32\drivers\etc\hosts* :

    localhost hlstest.com videos.com live.com
On  Linux, you have to add this line in the file */etc/hosts* :

    localhost hlstest.com videos.com live.com

## Test the demo :
To lauch the demo, you have to find a video (7-8 mn) and put it in the directory */var/www/html/*. Then, execute the following command :

    /var/www/html/create_vod.sh

This can be long, depending on your computer power.

In order to test VOD streaming, you have to open a browser and go to hlstest.com/demo/. Select **VOD Test** in the list.

In order to test Live streaming, you have to open a browser and go to hlstest.com/demo/. Select **Live Test** in the list. Then you have to click on the button *Active live*. 

The demonstration allows you to change the quality of the video, to see the logs, and modify few others parameters. 

That's it ! Well done !
