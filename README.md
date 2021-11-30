# harjoitus-5



## a) Watch it!

Otin yhteyden master vagrant koneelleni. Tein hakemiston `srv/salt/sshd` sinne kopion sshd_config komennolla `sudo cp sshd_config srv/salt/sshd` tiedoston hakemistosta `etc/ssh`

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

``
sudo salt '*' state.apply sshd
``
![image](https://user-images.githubusercontent.com/93308960/144125910-d1d0c9e3-5203-4fd9-9fe2-d39aa4071383.png)

komento meni läpi ja muutokset pitäisi tulla voimaan.

Yritin ottaa yhteyden orja koneeseni komennolla 

![image](https://user-images.githubusercontent.com/93308960/144120859-76905403-cc1e-479c-85e4-44773224605a.png)

![image](https://user-images.githubusercontent.com/93308960/144126493-e3e08f08-431f-4241-9d27-f368f28aca4a.png)



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

![image](https://user-images.githubusercontent.com/93308960/144072969-2c7bf144-d69a-40d2-9cad-6a9de30e56f0.png)



![image](https://user-images.githubusercontent.com/93308960/144073441-0bfe2f2e-ffe4-4a68-b996-5d5df56c1c2f.png)


![image](https://user-images.githubusercontent.com/93308960/144073635-e597f97c-6cf8-4106-a2a1-9f114399a5cd.png)



## b) Securerer shell

![image](https://user-images.githubusercontent.com/93308960/144125070-825245d1-c0f3-42e6-ac1d-72514dce8ba7.png)




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


