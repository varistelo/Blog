---
title: Swap em servidores Cloud
date: 2020-03-16
draft: true
tags:
  - cloud
  - memoria
  - swap
  - linux
categories:
  - tutoriais
ShowToc: true
TocOpen: true
---

Adicionar Memória Swap em Servidores Cloud

Testado em servidores Debian

`sudo fallocate -l 2G /swapfile`
`sudo chmod 600 /swapfile`
`sudo mkswap /swapfile`
`sudo swapon /swapfile`
`sudo swapon -s``

`#Editar fstab como root`
`sudo vim /etc/fstab`

`#Adicionar configuração ao final do arquvivo:`
`/swapfile none swap sw 0 0`

###### Fonte: [Create-swap-file](https://christitus.com/create-swap-file/)
