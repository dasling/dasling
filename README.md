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

If you want to take it for a spin, well please do.
Basically:
+ Install the Front-End (install nodejs and run dasling-FE)
    + sudo apt-get install nodejs (ubuntu), or http://nodejs.org/download/ (windows)
    + Download [dasling-FE](http://github.com/dasling/dasling-FE), and unzip the file, or even better: git clone https://github.com/dasling/dasling-FE.git
    + Enter in your database credentials in lib/config.js (need to match a user granted access on the DB, see steps below)
    + If you're using git, then better use filters to change your password, like so:
        + git config filter.password.clean "sed -e 's/yourpassword/@PASSWORD@/'"
        + git config filter.password.smudge "sed -e 's//@PASSWORD@/yourpassword/'"
        + make or edit .git/info/attributes to resemble:
        ```
        *.js filter=password
        ```
+ Install the DB (script included in dasling-FE)
    + cd into the db directory
    + mysql -u root -p
    + CREATE USER 'perpetual_pave'@'localhost' IDENTIFIED BY 'yourpassword'; #(or whatever user you want)
    + CREATE DATABASE perp_v1;
    + USE perp_v1
    + source db_schema.sql
    + source db_inserts.sql
    + GRANT ALL ON perp_v1.* TO 'perpetual_pave'@'localhost'; # or whatever user you choose
    + quit the mysql shell
    + mysql -u perpetual_pave -p # or whatever user you choose, and check whether the following can be done:
        + use perp_v1
        + select * from statuses; # this should provide a couple of standard statuses
+ Install the dasling MQTTjs server 
    + Download the dasling [MQQTjs](github.com/dasling/MQTT.js) or better: git clone https://github.com/dasling/MQTT.js.git
    + Add credentials to examples/server/dasling.js
    + If you're using git, then better use filters to change your password, like so:
        + git config filter.password.clean "sed -e 's/yourpassword/@PASSWORD@/'"
        + git config filter.password.smudge "sed -e 's//@PASSWORD@/yourpassword/'"
        + make or edit .git/info/attributes to resemble:
        ```
        *.js filter=password
        ```
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
+ Configure your devices/sensors/variables/actuators... in the Front-End
+ Check whether readings are stored in the DB
+ Write algorithms to perform calculations, and publish these to the dasling MQTTjs server
+ Check whether these to are stored in the DB
+ Subscribe your actuators to the configured MQTT topics
+ Enjoy ...

I'll add detailed instructions how to install dasling lateron.

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

Help needed?
------------
+ Lost in git: git-scm.com/book
