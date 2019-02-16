# Apache activemq docker image

Run the latest container with:

    docker pull paulohfcanuto/activemq
    docker run -p 61616:61616 -p 8161:8161 -p 5672:5672 -p 61613:61613 -p 1883:1883 -p 61614:61614 paulohfcanuto/activemq:5.15.8


Port Map
--------

    61616 JMS
    8161  UI
    5672  AMQP  
    61613 STOMP 
    1883  MQTT  
    61614 WS    


Customizing configuration and persistence location
--------------------------------------------------
By default data and configuration is stored inside the container and will be
lost after the container has been shut down and removed. To persist these
files you can mount these directories to directories on your host system:

    docker run -p 61616:61616 -p 8161:8161 \
               -v /your/persistent/dir/conf:/opt/activemq/conf \
               -v /your/persistent/dir/data:/opt/activemq/data \
               paulohfcanuto/activemq

ActiveMQ expects that some configuration files already exists, so they won't be
created automatically, instead you have to create them on your own before
starting the container. If you want to start with the default configuration you
can initialize your directories using some intermediate container:

    docker run --user root --rm -ti \
      -v /your/persistent/dir/conf:/mnt/conf \
      -v /your/persistent/dir/data:/mnt/data \
      paulohfcanuto/activemq:5.15.8 /bin/sh

This will bring up a shell, so you can just execute the following commands
inside this intermediate container to copy the default configuration to your
host directory:

    chown activemq:activemq /mnt/conf
    chown activemq:activemq /mnt/data
    cp -a /opt/activemq/conf/* /mnt/conf/
    cp -a /opt/activemq/data/* /mnt/data/
    exit

The last command will stop and remove the intermediate container. Your
directories are now initialized and you can run ActiveMQ as described above.


Credits to @rmohr