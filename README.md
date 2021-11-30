# harjoitus-5



## a) Watch it!




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




## c) Maailman suosituin


![image](https://user-images.githubusercontent.com/93308960/144079416-f9b8f92e-81fd-4f88-bbb7-1ae4d8a7583f.png)


![image](https://user-images.githubusercontent.com/93308960/144079488-eb0d8ea6-09d9-47dc-9d7d-5e45ca455ef0.png)



![image](https://user-images.githubusercontent.com/93308960/144081510-5005c32d-dd2d-4f1c-ac52-9db8f766e8b3.png)



## d) Minä ja kissani

![image](https://user-images.githubusercontent.com/93308960/144083522-470ff1d4-72b5-4875-895f-f5657b3e340f.png)



## e) Valmiiseen pöytään




## f) Mootorix
