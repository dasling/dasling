dasling
=======

dasling = Data Actuator Sensor (ling just expressing a sense of endearment towards this project).

dasling interconnects sensors and actuators through calculation steps and stores all this in a DB.

dasling allows you to:
+ log sensor readings to a database
+ abstraction sensor readings to variables
+ perform calculations on these variables (custom plug-in algorithms)
+ store the results of these calculation, or calculate further on these results, and so on
+ do calculation based on historical sensor data (and calculation results) 
+ send actions to actuators based on these calculations

dasling can be useful for a range of interesting stuff, e.g. interconnecting hardware around the globe, performing home automation, driving a led system, ...

Install:
--------

If you want to take it for a spin, well please do (if you would like a docker file, give me a ring).
Basically:
+ Install the Front-End (install nodejs and run dasling-FE)
    ```
    # Install nodejs & npm
    # if you don't have nodejs > 0.8, better download it from the nodejs website ! (and configure/make/make install it)
    sudo apt-get install nodejs npm # (ubuntu), or http://nodejs.org/download/ (windows)

    # Install jade templating engine for nodejs
    sudo npm install -g jade # don't know why, but jade is better installed global
    
    # Clone dasling-FE, but even better clone your own fork
    git clone https://github.com/dasling/dasling-FE.git # if you don't have git then: sudo apt-get install git
    
    # Enter the dasling-FE directory, and install some necessary nodejs modules
    cd dasling-FE
    npm install
    
    # Enter in your connection details in lib/config.js (need to match a user granted access on the DB, see steps below)
    cd dasling-FE/lib
    cp config_template.js config.js
    gedit config.js
    
    # Now make yourself a twitter account, cause you'll need it to authenticate yourself
    # surf to http://twitter.com
    # You'll need a twitter application: make it at dev.twitter.com/apps
    # Enter your consumerKey and consumerSecret in config.js
    gedit config.js    
    
    ```
+ Install the DB (script included in dasling-FE)
    ```
    # install mariaDB
    # instructions at https://downloads.mariadb.org/mariadb/repositories/

    # cd into the dasling-FE/db directory
    cd ./dasling-FE/db
    
    # login as root into mariaDB, and setup the database:
    mysql -u root -p # if you don't have mysql installed, see instruction on https://downloads.mariadb.org/mariadb/repositories
    CREATE USER 'dasling'@'localhost' IDENTIFIED BY 'yourpassword'; #(or whatever user you want)
    CREATE DATABASE dasling;
    USE dasling
    source db_schema.sql
    source db_inserts.sql
    source db_routines.sql   #first change the definer in file db_routines.js to the DB user chosen in config.js (see above)
    GRANT ALL ON dasling.* TO 'dasling'@'localhost';\G # or whatever user you choose
    quit
    
    # login as dasling user and check the database setup
    mysql -u dasling -p # or whatever user you choose, and check whether the following can be done:
        + use dasling
        + select * from statuses; # this should provide a couple of standard statuses
    ```
+ Install the dasling MQTTjs server
    ```
    # Download the dasling [MQQTjs](github.com/dasling/MQTT.js) or better: git clone
    git clone https://github.com/dasling/MQTT.js.git
     
    # install dependencies 
    cd dasling/MQTTjs
    npm install

    + Add credentials for database access
    cd examples/server/
    cp config_template.js config.js
    
    ```
+ Start dasling:
    + Run the mysql instance (if not automatically, if unsure, check in the processes: linux ps -A | grep -i mysql
    + Run the MQTTjs server:
        + cd MQQTjs/examples/server
        + node dasling.js
    + Run the dasling-FE
        + cd dasling-FE
        + node app.js
        + surf to localhost:3000
        + click on login -> authorize on twitter -> you should be send back to the application
        + Click on devices, if dasling complains about not finding 'id' of 'undefined' then logout and login, that resolves this issue permanently.
+ Install the hardware/equipment/...
    + The use a FLM (see flukso.net) (instructions mostly from www.flukso.net/content/what-development-environment-best-compile)
        + git clone https://github.com/dasling/flm02.git
        + git submodule init
        + git submodule update (if you have problems here, please set-up your github ssh keys coorectly)
        + sudo apt-get update
        + sudo apt-get install -y build-essential subversion git-core libncurses5-dev zlib1g-dev gawk flex quilt unzip gcc-avr avr-libc gdb-avr
        + sudo apt-get install -y avra avrp avrdude
        + cd ~/dasling/flm02
        + mkdir ./bin
        + cd openwrt
        + ./install.sh ~/dasling/flm02/bin
        + cd ~/dasling/flm02/bin/(whatever directory was made here)
        + make -j8 V=99
        + make sure the device is plugged in directly with a cross over cable and OFF. Not through a switch or router. When you see "Device detection in progress..", turn the device ON (plug power in) and it should be discovered in a few seconds and flashed in a couple of minutes.
        + make flash V=99 (if it complains about libpcap not found, then ap51-flash might be compiled for 32-bit. Better use open-mesh-flash-ng: Download it to /flm01/bin/flm02.x.x/tools/ ,then mv ap51-flash ap51-flash_32bit, then add a symlink to the new tool: ln -s ap51-flash open-mesh-flash-ng, finally chmod 755 open-mesh-flash-ng)
        + (Working hard on these instructions, but my FLMv2B is still bricked, so only do this when you know what you're doing, etc...)
            + Presumably, Problems are related to me having a 64-bit linux (related to openmesh or libraries used by it) 
+ Check with a simple mqqt message whether this works
    + download the [examples algorithms](http://github.com/dasling/dasling-example-algorithms), or git clone https://github.com/dasling/dasling-example-algorithms.git
    + add a password filtering, watch out filtering now set on *.py files
        + git config filter.password.clean "sed -e 's/yourpassword/@PASSWORD@/'"
        + git config filter.password.smudge "sed -e 's/@PASSWORD@/yourpassword/'"
        + make or edit .git/info/attributes to resemble:
        ```
        *.py filter=password
        ```
     +
+ Configure your devices/sensors/variables/actuators... in the Front-End
+ Check whether readings are stored in the DB (need to set store_in_DB field to '1' in Front-End for the variable)
+ Write algorithms to perform calculations, and publish these to the dasling MQTTjs server
+ Check whether these to are stored in the DB
+ Subscribe your actuators to the configured MQTT topics
+ Enjoy ...

License:
--------
dasling is licensed under GPL v2 or later.

Please make sure to check the license of the libraries and repositories used (or incorporated) in this project.

Similar projects:
-----------------
dasling was born out of necessity, cause similar projects exist, but typically not open source, or not what we needed:
+ Xively (Cosm, Puchube)
+ ThingSpeak
+ homA
+ open.sen.se
+ FHEM
+ ...

Difference: The intend is to have a dasling server at home which stores your info, and to publish/subscribe only those messages to a global dasling server which you don't mind to be public.
All this being open-source.


Help needed with git?
------------
+ Lost in git: git-scm.com/book


TODO:
-----
+ --DONE-- Add log from MQTT to DB when things go wrong
+ --DONE-- Add Attribute to "variable" specifying whether to store the reading/value in the DB
+ Add webpage for this log to FE 
+ Add webpage with lastest 20 (or x) MQTT messages coming in/out/republished
+ Add filters to the webpage with latest MQTT messages
+ Add support for groups (need to think this one through before we can begin, needs invites, etc ...)

ISSUES:
-------
+ On a fresh install, a 500 error is displayed (id = undefined) for all pages. Logout/login resolves this. 
