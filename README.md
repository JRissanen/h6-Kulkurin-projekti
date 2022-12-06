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
* "Vagrantfile":n sisältö:
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

* SSH:lla pääset kirjautumaan uuden virtuaalikoneen käyttäjälle:
  * `$ vagrant ssh t001`
* Ja pääset takaisin omaan käyttöjärjestelmääsi "exit" komennolla:
  * `vagrant@t001$ exit`

* Voit testata molempien koneiden ytheyttä Internettiin ja toisiinsa:
  * ```
     $ vagrant ssh t001
     vagrant@t001$ ping -c 1 192.168.88.102
     vagrant@t001$ ping -c 1 8.8.8.8 # Google nameserver
     vagrant@t001$ exit

     $ vagrant ssh t002
     vagrant@t002$ ping -c 1 192.168.88.101
     vagrant@t002$ exit
    ```

* Jos haluat tuhota molemmat virtuaalikoneet ja niiden kaikki tiedostot:
  * `$ vagrant destroy # all files on both virtual machines are destroyed`

* Uudet ja tyhjät virtuaalikoneet saat taas käyntiin:
  * `vagrant up`

* Joskus jotkin käyttöjärjestelmät antavat "IP numbers not allowed" virheilmoituksen.
* Kannattaa kokeilla vaihtaa "Vagrantfile":stä IP-osoitteen osoiteavaruus välille 56-63:
  * ```
     t001.vm.network "private_network", ip: "192.168.60.101"
     # ...
     t002.vm.network "private_network", ip: "192.168.60.102"
    ```

Karvinen 2018: Salt Quickstart – [Salt Stack Master and Slave on Ubuntu Linux](https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/)

Tiivistelmän tästä artikkelista voit lukea toisesta repositorystani, ensimmäisen alaotsikon alta: </br>
https://github.com/JRissanen/h2-Package-File-Service

#Tehtävät

Kaikki tehtävät on tehty Linux Ubuntu 22.04.1 LTS virtuaalikoneella, VirtualBoxin Versiolla 6.1.40.

__a) Hello Vagrant. Asenna virtuaalikone Vagrantilla.__

Aloitin näin: </br>
`sudo apt-get update`. </br>
Oma järjestelmäni kehotti myös päivittämään, joten: </br>
`sudo apt-get upgrade`. </br>

Sitten asensin Vagrantin ja Virtualboxin: </br>
`sudo apt-get -y install virtualbox vagrant`. </br>

Sitten yritin määrittää Vagrant-virtuaalikoneen: </br>

![Screenshot 2022-12-06 165218](https://user-images.githubusercontent.com/116954333/205945963-9090832d-0245-4016-a82f-72034bb87e64.png)

Mielestäni VirtualBox tukee sisäkkäistä virtualisointia, eli VirtualBoxilla tehdyn virtuaalikoneen tulisi myös pystyä asentamaan VirtualBox ja käyttämään sitä luomaan uusia virtuaalikoneita.

Aloitin vianselvityksen. </br>
Katsoin virheilmoitusta ja huomasin, että ongelma liittyi VirtualBox Manageriin, joten avasin sen GUI:ssa terminaalin viereen ja huomasin, että ainakin Vagrant oli onnistunut luomaan uuden virtuaalikoneen.

![Screenshot 2022-12-06 171516](https://user-images.githubusercontent.com/116954333/205950515-3b044053-5cc1-46cd-9b61-5e33b52d3b20.png)

Tajusin, etten ollut muutenkaan muistanut luoda harjoitusta varten omaa kansiota, joten poistin Vagrant-virtuaalikoneen: </br>
`vagrant destroy`. </br>
Poistin myös Vagrantfilen: </br>
`rm Vagrantfile`. </br>
Ja tein kotihakemistooni uuden hakemiston harjoituksille: </br>
`mkdir Vagrant/test_vagrant`.

Suljin virtuaalikoneeni ja aloitin etsimään netistä tietoa ongelmasta. </br>
Googletin ongelmaa virheilmoituksenmukaan: "virtualbox vt-x is not available ubuntu". </br>
Löysin keskutelupalstan: https://forums.virtualbox.org/viewtopic.php?f=7&t=101692 </br>
Sieltä löysin vinkin: </br>
"Kokeile laittaa VirtualBoxin isäntä koneen asetuksista: System > Processor > Enable Nested VT-x/AMD-V" päälle. </br>
Päätin kokeilla laittaa asetuksen "Enable Nested VT-x/AMD-V" päälle ja katsoa, jos se auttaisi.

![Screenshot 2022-12-06 175148](https://user-images.githubusercontent.com/116954333/205959219-c32a7c46-4559-4ad9-b7bf-1079e701c87b.png)

Käynnistin virtuaalikoneeni uudestaan ja koitin aloittaa nyt koko prosessin alusta. </br>
Alku näytti lupaavalta, koska uusi Vagrant-virtuaalikone lähti käyntiin: 

![Screenshot 2022-12-06 180623](https://user-images.githubusercontent.com/116954333/205962847-043ea289-378e-4bfd-b857-c289b74fef40.png)

Uuden Vagrant-virtuaalikoneen asennus oli kuitenkin niin hidasta, että asennus aikakatkaistiin:

![Screenshot 2022-12-06 181101](https://user-images.githubusercontent.com/116954333/205964156-52315f3a-53a4-4690-ac20-c2ac42eef9d4.png)

Kokeilin virheilmoituksen perusteella muuttaa "timeout" ajan arvoa. </br>
Avasin tiedoston: `micro Vagrantfile`. </br>
Poistin tiedostosta kaikki turhat kommentit. </br>
Lisäsin rivin: `config.vm.boot_timeout = 1000`. </br>
Ajattelin, että tuhat sekuntia olisi riittävän pitkä aika, jotta uusi virtuaalikone ehtisi lähteä käyntiin, ennen kuin yhteys katkaistaan.

![Screenshot 2022-12-06 184107](https://user-images.githubusercontent.com/116954333/205971068-5cc753e6-0ec2-4d7f-829d-060d3f715922.png)

Sitten annoin komennot `vagrant destroy` ja `vagrant up` kokeillakseni taas uudestaan.

![Screenshot 2022-12-06 185517](https://user-images.githubusercontent.com/116954333/205974150-772d60be-12c7-4c18-b8a4-95248b5ab7f0.png)

Kaiken tämän säädön jälkeen sain yhteyden muodostettua ja uuden Vagrant-virtuaalikoneen luotua, joten homma selvä. </br>

__b) Yksityisverkko. Asenna kaksi virtuaalikonetta samaan verkkoon Vagrantilla. Laita toisen koneen nimeksi "isanta" ja toisen "renki1". Kokeile, että "renki1" saa yhteyden koneeseen "isanta" (esim. ping tai nc). Tehtävä tulee siis tehdä alusta, vaikka olisit ehtinyt kokeilla tätä tunnilla.__






































Lähteet

https://terokarvinen.com/2017/04/11/vagrant-revisited-install-boot-new-virtual-machine-in-31-seconds/ </br>
https://terokarvinen.com/2021/two-machine-virtual-network-with-debian-11-bullseye-and-vagrant/ </br>
https://terokarvinen.com/2018/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/ </br>
https://forums.virtualbox.org/viewtopic.php?f=7&t=101692




