# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.11"
    config.vm.hostname = "privat.folbert.local"
    config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]

    # Optional NFS. Make sure to remove other synced_folder line too
    #config.vm.synced_folder ".", "/var/www", :nfs => { :mount_options => ["dmode=777","fmode=666"] }

	  config.vm.provision "shell", inline: <<-SHELL

        ## Elasticsearch install script found at
        ## https://qbox.io/blog/qbox-a-vagrant-virtual-machine-for-elasticsearch-2-x
        ##!/usr/bin/env bash
        #sudo apt-get update
        #sudo apt-get upgrade
        ## install openjdk-7
        #sudo apt-get purge openjdk*
        #sudo apt-get -y install openjdk-7-jdk
        ## install ES
        #wget -qO - https://packages.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
        #echo "deb http://packages.elastic.co/elasticsearch/2.x/debian stable main" | sudo tee -a /etc/apt/sources.list.d/elasticsearch-2.x.list
        #sudo apt-get update && sudo apt-get install elasticsearch
        #sudo update-rc.d elasticsearch defaults 95 10
        #sudo /etc/init.d/elasticsearch start
        ## either of the next two lines is needed to be able to access localhost:9200 from the host os
        #sudo echo "network.bind_host: 0" >> /etc/elasticsearch/elasticsearch.yml
        #sudo echo "network.host: 0.0.0.0" >> /etc/elasticsearch/elasticsearch.yml
        ## enable dynamic scripting
        #sudo echo "script.inline: on" >> /etc/elasticsearch/elasticsearch.yml
        #sudo echo "script.indexed: on" >> /etc/elasticsearch/elasticsearch.yml
        ## enable cors (to be able to use Sense)
        #sudo echo "http.cors.enabled: true" >> /etc/elasticsearch/elasticsearch.yml
        #sudo echo "http.cors.allow-origin: /https?:\/\/.*/" >> /etc/elasticsearch/elasticsearch.yml
        #sudo /etc/init.d/elasticsearch restart
        ## End of Elasticsearch install script

        # Update composer
        composer self-update

        # add composer to path
        export PATH="~/.composer/vendor/bin:$PATH"

        # change public to web to comply with Bedrock standards
        mv /var/www/public /var/www/web
        sudo sed -i s,/var/www/public,/var/www/web,g /etc/apache2/sites-available/000-default.conf
        sudo sed -i s,/var/www/public,/var/www/web,g /etc/apache2/sites-available/scotchbox.local.conf
        sudo service apache2 restart

	SHELL

end
