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

Tämäkään ei onnistunut yritin siivota `sshd_config` tiedoston. 
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



![image](https://user-images.githubusercontent.com/93308960/144133978-e052b97e-6b7c-4d23-88af-8f3de37681a8.png)



## c) Maailman suosituin


![image](https://user-images.githubusercontent.com/93308960/144079416-f9b8f92e-81fd-4f88-bbb7-1ae4d8a7583f.png)


![image](https://user-images.githubusercontent.com/93308960/144079488-eb0d8ea6-09d9-47dc-9d7d-5e45ca455ef0.png)



![image](https://user-images.githubusercontent.com/93308960/144081510-5005c32d-dd2d-4f1c-ac52-9db8f766e8b3.png)



## d) Minä ja kissani

![image](https://user-images.githubusercontent.com/93308960/144083522-470ff1d4-72b5-4875-895f-f5657b3e340f.png)

![image](https://user-images.githubusercontent.com/93308960/144084883-2b3ae5f7-c81f-481f-b9bb-997fea502a1a.png)



## e) Valmiiseen pöytään


![image](https://user-images.githubusercontent.com/93308960/144094172-4d255822-e830-4251-b743-3d9996a928c2.png)

![image](https://user-images.githubusercontent.com/93308960/144106823-aa87bbfa-9293-46b2-9efc-89a72c1898c0.png)



## f) Mootorix


![image](https://user-images.githubusercontent.com/93308960/144108114-b907900f-af08-4f55-a925-d733062b1c45.png)


![image](https://user-images.githubusercontent.com/93308960/144110485-1d074e49-4e5a-4881-84f0-2127d6c24083.png)

![image](https://user-images.githubusercontent.com/93308960/144113014-2fdb51d1-5dca-421d-9a91-552b4014ac51.png)


