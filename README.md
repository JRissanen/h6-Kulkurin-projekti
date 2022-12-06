# h6-Kulkurin-projekti
This is a repository for Configuration Management Systems - Palvelinten Hallinta course's exercise 6

__x) Lue ja tiivistä (muutamalla ranskalaisella viivalla per artikkeli, poimi esim itsellesi keskeisimmät komennot)__

Karvinen 2017: [Vagrant Revisited – Install & Boot New Virtual Machine in 31 seconds](https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/) (Suosittelen käyttämään tässä koneena 'vagrant init debian/bullseye64')

* Pikaohje kuinka käyttää Vagrant-virtuaalikoneita Linux koneella.
* Vagrant asentaa uuden virtuaalikoneen automaattisesti.
* Koneita hallitaan SSH-yhteyden avulla.

* Ensin pitää asentaa Vagrant ja VirtualBox:
  * ```
     $ sudo apt-get update
     $ sudo apt-get -y install vagrant virtualbox
    ```
* Vagrant-virtuaalikoneet hyödyntävät VirtualBoxin virtuaaliympäristöä, siksi myös VirtualBox pitää asentaa.

* Uuden Vagrant-virtuaali koneen luominen:
   * ```
     $ vagrant init bento/ubuntu-16.04
     $ vagrant up
     $ vagrant ssh
     ```

* Jos haluat tuhota Vagrant-virtuaalikoneen ja sen kaikki tiedostot:
  * `$ vagrant destroy`

Karvinen 2021: [Two Machine Virtual Network With Debian 11 Bullseye and Vagrant](https://terokarvinen.com/2022/palvelinten-hallinta-2022p2/?from=MoodleNews#h1-hello-salt)

* Vagrantin käyttöohje.
* Vagrant:
 * Automaattisesti asentaa uuden virtuaalikoneen.
 * Automatisoi SSH sisäänkirjautumisen.
 * Graafista käyttöliittymää ei tarvita.
* Vagrantin ja VirtualBoxin asennus: (Vagrant vaatii toimiakseen VirtualBoxin.)
  * ```
     $ sudo apt-get update
     $ sudo apt-get -y install vagrant virtualbox
    ```
    
* Kuinka luoda kaksi Vagrant-virtuaalikonetta:
* Tee ensin uuttaprojektia varten uusi hakemisto.
 * `$ mkdir twohost/; cd twohost/`
* Tee ja tallenna hakemistoon "Vagrantfile", joka määrittää Vagrantin toiminnan:
 * `$ nano Vagrantfile`
* Vagrantfile:n sisältö:
* ```
  # -*- mode: ruby -*-
  # vi: set ft=ruby :
  # Copyright 2019-2021 Tero Karvinen http://TeroKarvinen.com

  $tscript = <<TSCRIPT
  set -o verbose
  apt-get update
  apt-get -y install tree
  echo "Done - set up test environment - https://terokarvinen.com/search/?q=vagrant"
  TSCRIPT

  Vagrant.configure("2") do |config|
	  config.vm.synced_folder ".", "/vagrant", disabled: true
	  config.vm.synced_folder "shared/", "/home/vagrant/shared", create: true
	  config.vm.provision "shell", inline: $tscript
	  config.vm.box = "debian/bullseye64"

	  config.vm.define "t001" do |t001|
		  t001.vm.hostname = "t001"
		  t001.vm.network "private_network", ip: "192.168.88.101"
	  end

	  config.vm.define "t002", primary: true do |t002|
		  t002.vm.hostname = "t002"
		  t002.vm.network "private_network", ip: "192.168.88.102"
	  end
	
  end
  ```

* 


























Lähteet

https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/
https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/
https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/





