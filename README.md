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

