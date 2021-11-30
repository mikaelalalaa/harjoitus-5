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





## d) Minä ja kissani



## e) Valmiiseen pöytään




## f) Mootorix
