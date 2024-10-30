Abdirahman Mire	Raportti	**4** (4)

Haaga-Helia ammattikorkeakoulu	30.10.2024

ICI001AS3A-3010


# **H1 Viisikko**

## Taustatiedot: 
Tehtävässä harjoittelin SaltStackin käyttöä Linuxissa.
## Työympäristö:
Tehtävän tekoon käytin kotonani olevaa pc:tä: 

- Käyttöjärjestelmä: Windows 10 Pro 64-bit
- Suoritin: Intel Core i7-4790 
- Muisti: 12GB DDR3
- Näytönohjain: NVIDIA GeForce GTX 1050 Ti

Virtuaalinen kone Virtualboxin kautta johon asennettuna Debian 12 Bookworm.
## X) Tiivistelmä
- ` `Saltin avulla voi paikallisesti hallita suurta määrää tietokoneita verkon yli. Yleensä järjestelmä koostuu yhdestä master-palvelimesta ja useista minion-palvelimista.
- Tärkeitä Salt-toimintoja: pkg (sovellusten asennus/poisto), file (tiedostojen hallinta), service (palveluiden hallinta), user (käyttäjien hallinta), cmd (komentojen suorittaminen)
- Raportissa täytyy mainita tarkat toimeenpiteet, komennot ja ympäristö. Raportin kuuluu olla helppolukuinen ja pitää muistaa viittaa kaikkiin lähteisiin.

## B)  Salt asennus
kellonaika: 9:38, tehtävään kului noin muutama minuutti.

Sain Saltin minionin asennettua käyttäen seuraavia komentoja:

- sudo apt-get update
- sudo apt-get -y install salt-minion

Tämän jälkeen tarkistin asennuksen komennolla:

- sudo salt-call –version



## C) Tärkeät komennot
Kellonaika: 9:46, aikaa kului noin 1h

- **pkg**

käytin komentoa: 

sudo salt-call --local -l info state.single pkg.installed tree

Tämä asensi tree paketin

ja komennolla: 

sudo salt-call --local -l info state.single pkg.removed tree

sain poistettua tree paketin. 

- **file**

Käytin komentoa: 

sudo salt-call --local -l info state.single file.managed /home/mire/test contents="Hello, Salt!"

Tämä loi tiedoston /home/mire/test ja kirjoitti siihen Hello, Salt! 


- **service**

Service komentoa kokeilin service.runnning komennolla:

sudo salt-call --local -l info state.single service.running name=apache2 enable=True

tämä käynnisti apache2 palvelun ja varmistaa että palvelu käynnistyy automaattisesti, ja päin vastoin service.dead lopettaa palvelun ja estää automaattisen käynnistyksen komennolla:

` `sudo salt-call --local -l info state.single service.dead name=apache2 enable=True

- **user**

loin uuden käyttäjän käyttäen komentoa: 

sudo salt-call --local -l info state.single user.present testuser

- **cmd**

Käytin komentoa: 

sudo salt-call --local -l info state.single cmd.run 'touch /home/mire/test2’ creates='/home/mire/test2’

tässä komennossa ‘touch /home/mire/test2’ loi tiedoston ’creates’ komennon tuloksen perusteella, eli creates tarkistaa tiedoston olemassaolon ennen touch komennon suorittamista.  

## D) Idempotenssi

esimerkiksi valitsin komennon:

sudo salt-call -local -I info state.single pkg.installed tree

suoritettuani tämän kerran asensi tree paketin, kun suoritin tämän uudestaan saavutin idempotenssi tilan sillä ei tule muutoksia koska paketti on jo asennettu. 

## E) Herra-orja arkkiteehtuuri
kello: 11:20 aikaa kului noin 10 minuuttia

Salt-minion oli jo asennettuna joten asensin salt-masterin komennolla:

sudo apt-get install salt-master

Tämän jälkeen muokkasin minionin konfiguraatiotiedosta siten että määritin sinne masterin osoitteen: 10.0.2.15 ja lisäsin id:n minionille: my-minion. Tämä jälkeen uudelleenkäynnistin minionin ja masterin, jotta he saisivat yhteyden komennola: 

sudo systemctl restart salt-minion.service

Tämän jälkeen tarkistin onko minion rekisteröitynyt komennolla:

sudo salt-key -L

Hyväksyin avaimen komennolla: 

sudo salt-key -a my-minion

Jonka jälkeen kokeilin yhteyttä onnistuneesti komennolla:

sudo salt '\*' cmd.run 'whoami'

## Viiteet ja lähteet: 
<https://terokarvinen.com/palvelinten-hallinta/>

<https://terokarvinen.com/2021/salt-run-command-locally/>

<https://terokarvinen.com/2018/03/28/salt-quickstart-salt-stack-master-and-slave-on-ubuntu-linux/>

https://terokarvinen.com/2021/install-debian-on-virtualbox/

- “Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. <http://www.gnu.org/licenses/gpl.html>”
- “Pohjana Tero Karvinen 2012: [Linux kurssi, http://terokarvinen.com](http://terokarvinen.com/)”








