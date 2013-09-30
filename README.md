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
+ Install the DB (script included in dasling-FE)
  ++ Download [db_schema.sql](github.com/dasling/dasling-FE/tree/master/db)
  ++ mysql -u root -p
  ++ CREATE DATABASE perp_v1
  ++ USE perp_v1
  ++ source db_schema.sql
+ Install the Front-End (install nodejs and run dasling-FE)
+ Install the dasling MQTTjs server 
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
