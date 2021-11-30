# harjoitus-5



## a) Watch it!

Otin yhteyden master vagrant koneelleni. Tein hakemiston `srv/salt/sshd` sinne kopion sshd_config tiedoston komennolla `sudo cp sshd_config srv/salt/sshd`  hakemistosta `etc/ssh`

`srv/salt/sshd/` hakemistossa tein uuden tiedoston `init.sls` johon kirjoitin alla olevan tekstin.

```

openssh-server:
  pkg.installed

/etc/ssh/sshd_config:
  file.managed:
    - source: salt://sshd/sshd_config

sshd:
  service.running:
    - enable: True
    - watch:
      - file: /etc/ssh/sshd_config

```

Tämän jälkeen ajoin komennon, jossa samalla vaihoin portin numeroksi 2222

```
sudo salt '*' state.apply sshd
```

![image](https://user-images.githubusercontent.com/93308960/144127642-38d83c34-5162-4793-b361-105a0790fe9a.png)

komento meni läpi ja muutokset pitäisi tulla voimaan.

Yritin ottaa yhteyden orja koneeseni komennolla `sudo ssh vagrant@iposoite`

Mutta tämä ei onnistunut ja kysyttiin ssh julkista avainta.

![image](https://user-images.githubusercontent.com/93308960/144120859-76905403-cc1e-479c-85e4-44773224605a.png)

Sitten alettiin googlailee ja katsomaan.

Yritin eri komentoja kuten 

```
ssh-keygen
&
ssh-agent sh -c 'ssh-add; ssh-add -L'

```

Ensimmäisellä komennolla `ssh-keygen` yritin luoda ssh avainta no se kaipa onnistu 

![image](https://user-images.githubusercontent.com/93308960/144128476-122d9d75-94a6-46bc-b34f-03030ae9fb7d.png)

toinen komento `ssh-agent sh -c 'ssh-add; ssh-add -L'` näyttää ssh avaimen

yritin uudestaan ottaa yhteyttä orja koneeseen mutta herjasi samaa juttua

Niiden jälkeen yritin kopioida avaimia toisin puolin komennolla `ssh-copy-id -i` no mutta eipähän onnistunu.

Yritin myös ottaa yhteyttä akuperäsellä portilla 22 mutta ei sekään onnistunt. 
![image](https://user-images.githubusercontent.com/93308960/144126493-e3e08f08-431f-4241-9d27-f368f28aca4a.png)

Päätin sitten näyttää toisella tavalla että voin muokata ssh konfiguraatio tiedostoa saltin avulla.

Todistan sen virtuaali koneellani ottamalla puttyyn yhteyttä muokatulla 2222 portilla.

Aloitin samanlailla luomalla hakemiston `srv/salt/sshd` sinne kopion `sshd_config` tiedoston komennolla `sudo cp sshd_config srv/salt/sshd`  hakemistosta `etc/ssh`.

Hakemistoon `srv/salt/sshd` loin `init.sls` tiedoston ja lisäsin alla olevan tekstin.


```

openssh-server:
  pkg.installed

/etc/ssh/sshd_config:
  file.managed:
    - source: salt://sshd/sshd_config

sshd:
  service.running:
    - enable: True
    - watch:
      - file: /etc/ssh/sshd_config

```

Ajoin komennon 

```
sudo salt '*' state.apply sshd
```


Kuten alla olevasta kuvasta *(oikea puoli)* näkyy ssh on asennettu jo aikasemmin ja portin vaihto onnistui. Avasin puttyn *(vasen puoli)* ja kirjoitin ip osoitteeni `Host Name` kohtaan ja portti oli 2222, sitten otetaa yhteyttä `open` kohdasta

![image](https://user-images.githubusercontent.com/93308960/144073441-0bfe2f2e-ffe4-4a68-b996-5d5df56c1c2f.png)

Alhaasta olevasta kuvasta näkyy että yhetys oli onnistunut

![image](https://user-images.githubusercontent.com/93308960/144073635-e597f97c-6cf8-4106-a2a1-9f114399a5cd.png)



## b) Securerer shell

Siivosin `sshd_config` tiedoston. 
Ensin komennolla 

```
cat sshd_config |grep -v ^#|grep -v ^$

```

Jotta näkee miltä siivous näyttäisi. Sitten yritän siirtää sitä uuteen tiedostoon komennolla 

```
cat ssshd_config |grep -v ^#|grep -v ^$ > sudo tee sshd_config

```

Alla olevasta kuvasta näkyy että onnistui

![image](https://user-images.githubusercontent.com/93308960/144133638-3410f9a4-a873-464a-ba0d-320f1b1e6382.png)
 

Loin hakemistoon `etc/ssh` banner.txt tiesoton ja kopion sen myös `srv/salt/sshd` hakemistoon.
Jonka jälkeen muokkasin hakemistossa `srv/salt/sshd` olevaa `init.sls` tiedostoa ja lisäsin rivin 

```

banner:
  file.managed:
    - source: salt://sshd/banner.txt
    - name: /etc/ssh/banner.txt


```

Tämä jälkeen ajoin komennon:

```
sudo salt '*' state.apply sshd
```

Alla olevasta kuvasta näkyy että muutokset tuli voimaan.

![image](https://user-images.githubusercontent.com/93308960/144133978-e052b97e-6b7c-4d23-88af-8f3de37681a8.png)

Tässä vielä näkyy että tervehdys tuli.

![image](https://user-images.githubusercontent.com/93308960/144134332-fe584973-29e6-4300-ba6e-27ee809444bd.png)



## c) Maailman suosituin

Loin uuden hakemiston `srv/salt/apache2` johon loin `init.sls` tiedoston.

Laitoin alla olevan tekstin tiedostoon

```
apache2:
  pkg.installed

newfile:
  file.managed:
    - source: salt://apache/index.html
    - name: /var/www/html/index.html

apacheservice:
  service.running:
    - name: apache2

```

Sitten loin `index.html` tiedoston johon laitoin alla olevan tekstin.

![image](https://user-images.githubusercontent.com/93308960/144135896-25ef4db4-f5d5-42fd-bb59-ccec7d8fcfda.png)

Ajoin komennon 

```
sudo salt-call --local state.apply apache2
```

Alla olevasta kuvasta näkyy että apache2 asetnui

![image](https://user-images.githubusercontent.com/93308960/144079416-f9b8f92e-81fd-4f88-bbb7-1ae4d8a7583f.png)

Mutta file.amanged kohta ei mennyt läpi.

![image](https://user-images.githubusercontent.com/93308960/144079488-eb0d8ea6-09d9-47dc-9d7d-5e45ca455ef0.png)


Kävin katsomassa `init.sls` tiedostoa ja huomasin että oli tullut kirjoitus virhe `newfile` kohtaan.

Koodi muokattiin `source` kohdasta eli `salt://apache/index.html` --> `salt://apache2/index.html`, apache kohdasta oli jäänyt numero kakkonen.

```
apache2:
  pkg.installed

newfile:
  file.managed:
    - source: salt://apache2/index.html
    - name: /var/www/html/index.html

apacheservice:
  service.running:
    - name: apache2

```

Ajoin uudestaan komennon 

```
sudo salt-call --local state.apply apache2
```

Tällä kertaa kaikki meni onnistuneestin läpi kuten kuvassa näkyy.

![image](https://user-images.githubusercontent.com/93308960/144081510-5005c32d-dd2d-4f1c-ac52-9db8f766e8b3.png)

Isolla näkyy selaimella luoma `index.html` sivu ja oikealla ikkunassa näkyy salt komennon tulokset


## d) Minä ja kissani

Otin userdir käyttöön komennolla `sudo a2enmod userdir`, sitten ajoin aikajan komennon eli 

```
cd /etc/; sudo find -printf '%T+ %p\n'|sort|tail
```
Alla kuvassa näkyy kaksi vikaa riviä missä näkyy että muutokset tuli voimaan 

![image](https://user-images.githubusercontent.com/93308960/144083522-470ff1d4-72b5-4875-895f-f5657b3e340f.png)

Sitten menin `srv/salt/sshd` hakemistoon ja muokkasin `init.sls` tiedostoa alla olevan kuvan mukaan.

![image](https://user-images.githubusercontent.com/93308960/144137262-ad2e0279-39af-4394-92f6-909a418031ee.png)

Lisäämällä kaksi riviä jottai voin luoda file.symlinkit

```
/etc/apache2/mods-enabled/userdir.conf:
  file.symlink:
    - target: /etc/apache2/mods-enable/userdir.conf

/etc/apache2/mods-enabled/userdir.load:
  file.symlink:
    - target: /etc/apache2/mods-enabled/userdir.load
 - watch:
      - file: /etc/apache2/mods-enabled/userdir.conf
      - file: /etc/apache2/mods-enabled/userdir.load

```

Ajoin komennon `sudo salt '*' state.apply apache2` ja kuten kuvassa näkyy muutokset onnistui 

![image](https://user-images.githubusercontent.com/93308960/144084883-2b3ae5f7-c81f-481f-b9bb-997fea502a1a.png)



## e) Valmiiseen pöytään

Loin hakemiston `srv/salt/copy`. Sen jälkeen menin hakemistoon `/etc/skel/` johon loin uuden kansion komennolla `sudo mkdir public_html` ja sinne tiedoston `index.html`.

Tiedostoon laitoin alla olevan koodin

![image](https://user-images.githubusercontent.com/93308960/144138182-c4c73031-f412-4e4f-8f55-0b24b21bc5ed.png)

Tämän jälkeen kopioin `index.html` tiedoston `srv/salt/copy` hakemistoon. Komento oli `sudo cp index.html srv/salt/copy`. Siirryin myös `srv/salt/copy` hakemistton ja loin sinne `init.sls` tiedoston. 

Kirjoitin alla olevan tekstin vasta luotuun tiedostoon.

```
/etc/skel/public_html/index.html:
  file.managed:
    - source: salt://copy/index.html
    - makedirs: True
```

Sitten ajoin komennon

```
sudo salt-call --local state.apply copy
```

Muutokset onnistui

![image](https://user-images.githubusercontent.com/93308960/144094172-4d255822-e830-4251-b743-3d9996a928c2.png)

Testasin että onnistuko kunnolla, luomalla uuden käyttäjän komennolla `sudo adduser jay`.
Sen jälkeen testasin tuliko  html tiedosto automaattisestin ja ajoin komennon `cat home/jay/public_html/index.html` 

Kuvasta näkyy että onnistui.

![image](https://user-images.githubusercontent.com/93308960/144106823-aa87bbfa-9293-46b2-9efc-89a72c1898c0.png)



## f) Mootorix

Ensin asensin Nginx sovelluksen käsin komennoilla

```
sudo apt update
&
sudo apt install nginx

```

Katsoin että Nginx on käynnissä komennolla `systemctl status nginx`

![image](https://user-images.githubusercontent.com/93308960/144108114-b907900f-af08-4f55-a925-d733062b1c45.png)

Loin uuden hakemiston `srv/salt/nginx` johon loin `init.sls` ja `index.nginx-debian.html` tiedostot.

init tiedostoon laitoin alla olevan tekstin

```

nginx:
  pkg.installed

/var/www/html/index.nginx-debian.html:
  file.managed:
    - source: salt://nginx/index.nginx-debian.html
```

Alla olevasta kuvasta näkyy mitä kirjoitin html tiedostoon

![image](https://user-images.githubusercontent.com/93308960/144139933-5de4c732-6593-4e9f-9ef5-d7171beba256.png)

Sitten ajoin komennon 

```
sudo salt '*' stata.apply nginx
```

Alla olevasta kuvasta näkyy komennon tulokset *(oiken puolinen ikkuna)*, kaikki muutokset tuli onnistuneestin käyttön.
Kuvasta näkyy nyös selaimen jossa testasin luomaani html tiedostoa.

![image](https://user-images.githubusercontent.com/93308960/144113014-2fdb51d1-5dca-421d-9a91-552b4014ac51.png)

Kaikki toimi.


## lähteet

https://www.digitalocean.com/community/tutorials/how-to-install-nginx-on-ubuntu-20-04

https://askubuntu.com/questions/311558/ssh-permission-denied-publickey

https://jhooq.com/vagrant-copy-public-key/

https://terokarvinen.com/2021/configuration-management-systems-palvelinten-hallinta-ict4tn022-2021-autumn/#h5-demonit
