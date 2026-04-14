# <div align="center"><strong>МОДУЛЬ 1</strong></div>

<br/>

<p align="center">
    <img width="504" height="465" alt="image" src="https://github.com/user-attachments/assets/456ac6dd-4d0b-4f76-9863-aa41076179c3" />
  </a>
</p>

---

<a id="Первоначальная настройка"></a>
## Первоначальная настройка

<details>
<summary><strong>Первоначальная настройка всех машин</strong></summary>
  
```
nano /etc/apt/sources.list
```

Комментируем строчку

<img width="890" height="617" alt="image" src="https://github.com/user-attachments/assets/8a0d7247-4f60-4ced-8953-ae1a6a9c0009" />

Дальше идем в resolv.conf

```
nano /etc/resolv.conf
```
Удаляем там всё и прописываем

```
nameserver 8.8.8.8
```

<img width="300" height="84" alt="image" src="https://github.com/user-attachments/assets/d7e0c1b1-facc-4978-b38e-afdbaaf10893" />

Теперь идем в файл sysctl.conf
```
nano /etc/sysctl.conf
```
Надо раскоментировать и включить параметр (Поменять 0 на 1)
```
net.ipv4.ip_forward=1
```

<img width="271" height="197" alt="image" src="https://github.com/user-attachments/assets/84238f1f-da09-4666-8e81-5612c893b822" />

Теперь применяем данные 
```
sysctl -p
```
</details>

---

<a id="именаустройств"></a>
## Задание 1. Настройка имен устройств

<details>
<summary>Описание</summary>
Настройте имена устройств согласно топологии. Используйте полное доменное имя
</details>

<details>
<summary><strong>Решение</strong></summary>

> <strong>ISP</strong>:
```
hostnamectl set-hostname isp.au-team.irpo; exec bash
```

> <strong>HQ-RTR</strong>:
```
hostnamectl set-hostname hq-rtr.au-team.irpo; exec bash
```

> <strong>BR-RTR</strong>:
```
hostnamectl set-hostname br-rtr.au-team.irpo; exec bash
```

> <strong>HQ-SRV</strong>:
```
hostnamectl set-hostname hq-srv.au-team.irpo; exec bash
```

> <strong>HQ-CLI</strong>:
```
hostnamectl set-hostname hq-cli.au-team.irpo; exec bash
```

> <strong>BR-SRV</strong>:
```
hostnamectl set-hostname br-srv.au-team.irpo; exec bash
```
</details>

---

<a id="Настройка адресации"></a>
## Настройка адресации

<details>
<summary>Задание</summary>
На всех устройствах необходимо сконфигурировать IPv4:
  
- IP-адрес должен быть из приватного диапазона, в случае, если сеть локальная, согласно RFC1918  
- Локальная сеть в сторону HQ-SRV(VLAN 100) должна вмещать не более 32 адресов  
- Локальная сеть в сторону HQ-CLI(VLAN 200) должна вмещать не менее 16 адресов  
- Локальная сеть для управления(VLAN 999) должна вмещать не более 8 адресов  
- Локальная сеть в сторону BR-SRV должна вмещать не более 16 адресов  
- Настройте адресацию на интерфейсах:  
- Интерфейс, подключенный к магистральному провайдеру, получает адрес по DHCP  
- Настройте маршрут по умолчанию, если это необходимо  
- Настройте интерфейс, в сторону HQ-RTR, интерфейс подключен к сети 172.16.1.0/28  
- Настройте интерфейс, в сторону BR-RTR, интерфейс подключен к сети 172.16.2.0/28  
</details>

<details>
<summary><strong>Настройка адрессации</strong></summary>

### ISP

```
nano /etc/network/interfaces
```

```
auto ens192
iface ens192 inet dhcp

auto ens224
iface ens224 inet static
address 172.16.1.1/28

auto ens256
iface ens256 inet static
address 172.16.2.1/28
```

<img width="817" height="606" alt="image" src="https://github.com/user-attachments/assets/048f66a6-2674-4826-ad32-db410c31a04d" />

```
systemctl restart networking
```

```
ip -c a
```

### HQ-RTR

```
nano /etc/network/interfaces
```

```
auto ens192
iface ens192 inet static
address 172.16.1.2/28
gatewat 172.16.1.1

auto ens224
iface ens224 inet static
address 192.168.1.1/28
```

<img width="767" height="611" alt="image" src="https://github.com/user-attachments/assets/48a24e6d-abf8-4239-b08a-e317c685fa51" />

```
systemctl restart networking
```

```
ip -c a
```

### BR-RTR

```
nano /etc/network/interfaces
```

```
auto ens192
iface ens92 inet static
address 172.16.2.2/28
gatewat 172.16.2.1

auto ens224
iface ens 224 inet static
address 192.168.4.1/28
```

<img width="753" height="590" alt="image" src="https://github.com/user-attachments/assets/0fedfc67-b809-47d8-a0a0-07e9c230caf6" />

```
systemctl restart networking
```

```
ip -c a
```

### HQ-SRV

```
nano /etc/network/interfaces
```

```
auto ens192
iface ens192 inet static
address 192.168.1.2/27
gateway 192.168.1.1
```

<img width="793" height="590" alt="image" src="https://github.com/user-attachments/assets/36cac43d-5392-4f75-b2b2-d99ec34f50c2" />

```
systemctl restart networking
```

```
ip -c a
```

### HQ-CLI

```
nano /etc/network/interfaces
```

```
auto ens192
iface ens192 inet dhcp
```

```
systemctl restart networking
```

```
ip -c a
```

### BR-SRV

```
nano /etc/network/interfaces
```

```
auto ens192
iface ens192 inet static
address 192.168.4.2/28
gateway 192.168.4.1
```
<img width="809" height="602" alt="image" src="https://github.com/user-attachments/assets/956008e1-bd6c-4e3d-99f9-cb328781e9b0" />


```
systemctl restart networking
```

```
ip -c a
```

</details>

<a id="nat"></a>
## 3.Настройка NAT на ISP

<details>
<summary>Задание</summary>
На ISP настройте динамическую сетевую трансляцию портов для доступа к сети Интернет HQ-RTR и BR-RTR.
</details>

<details>
<summary>Данное задание выполняеться на <strong>ISP</strong> машине</summary>

Настраиваем ISP в сторону HQ-RTR и BR-RTR и nftables для доступа к интернету

### ISP

```
nano /etc/nftables.conf
```

в конце файла добавить правило

```
table inet nat  {
  chain POSTROUTING {
  type nat hook postrouting priority srcnat;
  oifname "ens192" masquerade
  }
}
```
<img width="803" height="549" alt="image" src="https://github.com/user-attachments/assets/7ad4a0eb-cc1e-4e1d-8f88-79b344217141" />

Перезагружаем
```
systemctl restart nftables
```

Добавляем в автозагрузку
```
systemctl enable --now nftables
```

Проверка на HQ-RTR

```
ping 8.8.8.8
```

<img width="637" height="255" alt="image" src="https://github.com/user-attachments/assets/4d837380-c34e-4bca-aa1c-a274cab23c14" />

Проверка на BR-RTR

```
ping 8.8.8.8
```

<img width="673" height="242" alt="image" src="https://github.com/user-attachments/assets/a917a0a9-3857-4cee-b90d-fd3199192b1a" />

<a id="пользователи"></a>
##  4. Создание пользоватлей

<details>
<summary>Задание</summary>
Создайте локальные учетные записи на серверах HQ-SRV и BR-SRV:
  
- Создайте пользователя sshuser
- Пароль пользователя sshuser с паролем P@ssw0rd
- Идентификатор пользователя 2026
- Пользователь sshuser должен иметь возможность запускать sudo без ввода пароля
- Создайте пользователя net_admin на маршрутизаторах HQ-RTR и BRRTR
- Пароль пользователя net_admin с паролем P@ssw0rd
</details>

<details>
<summary>Задание выполняется на машинах <strong>HQ-SRV</strong>, <strong>BR-SRV</strong>, <strong>HQ-RTR</strong> и <strong>BR-RTR</strong></summary>
### HQ-SRV

Создание пользователя с иденфикатором 2026

```
useradd sshuser -u 2026 -U
```

Cтавим пароль `P@ssw0rd`

```
passwd sshuser
```

Далее нужно выдать права root

```
usermod -aG sudo sshuser
```

Далее переходим в конфигурационный файл пользователей

```
nano /etc/sudoers
```

Прописываем там пользователя

```
sshuser  ALL=(ALL:ALL) NOPASSWD:ALL
```

<img width="795" height="609" alt="image" src="https://github.com/user-attachments/assets/100cf888-6739-4fce-9e4d-272dd9216abe" />

### BR-SRV

Создание пользователя с иденфикатором 2026

```
useradd sshuser -u 2026 -U
```

Cтавим пароль `P@ssw0rd`

```
passwd sshuser
```

Далее нужно выдать права root

```
usermod -aG sudo sshuser
```

Далее переходим в конфигурационный файл пользователей

```
nano /etc/sudoers
```

Прописываем там пользователя

```
sshuser  ALL=(ALL:ALL) NOPASSWD:ALL
```

<img width="852" height="627" alt="image" src="https://github.com/user-attachments/assets/e3e18e3f-6f70-41d6-90c8-8893be30b39d" />

### HQ-RTR

Создаем пользователя

```
useradd net_admin
```

Ставим пароль командой passwd

```
passwd net_admin
```

Выдаем права

```
usermod -aG sudo net_admin
```

```
nano /etc/sudoers
```

Прописываем там пользователя

```
net_admin ALL=(ALL:ALL) NOPASSWD:ALL
```

<img width="745" height="599" alt="image" src="https://github.com/user-attachments/assets/5d547dbe-23e4-4a47-a8db-282047e73cf8" />

### BR-RTR

Создаем пользователя

```
useradd net_admin
```

Ставим пароль командой passwd

```
passwd net_admin
```

Выдаем права

```
usermod -aG sudo net_admin
```

```
nano /etc/sudoers
```

Прописываем там пользователя

```
net_admin ALL=(ALL:ALL) NOPASSWD:ALL
```

<img width="807" height="625" alt="image" src="https://github.com/user-attachments/assets/75f80329-4711-48be-b4f6-1b2bae464dbf" />

</details>

<a id="vlan"></a>
## 5. Настройка VLAN

<details>
<summary>Задание</summary>
Настройте коммутацию в сегменте HQ следующим образом: 
  
- Трафик HQ-SRV должен принадлежать VLAN 100
- Трафик HQ-CLI должен принадлежать VLAN 200
- Предусмотреть возможность передачи трафика управления в VLAN 999
- Реализовать на HQ-RTR маршрутизацию трафика всех указанных VLAN с использованием одного сетевого адаптера ВМ/физического порта
- Сведения о настройке коммутации внесите в отчёт
</details>

<details>
<summary>Данное задание выполняеться на <strong>HQ-RTR</strong></summary>

- Настройте коммутацию в сегменте HQ следующим образом:
- Трафик HQ-SRV должен принадлежать VLAN 100
- Трафик HQ-CLI должен принадлежать VLAN 200
- Предусмотреть возможность передачи трафика управления в VLAN 999</br>

Установка адресации на VLAN

На HQ-RTR НУЖНО ПРОВЕРИТЬ ФАЙЛ /etc/resolv.conf
```
nameserver 8.8.8.8
```
Устанавливаем пакет Vlan

```
apt install vlan -y
```

Далее добавляем модуль 8021q

```
echo 8021q >> /etc/modules
```

Дальше идем в интерфейсы

```
nano /etc/network/interfaces
```

```
auto ens192  
iface ens192 inet static  
address 172.16.1.2/28    
gateway 172.16.4.1  
  
auto ens224  
iface ens224 inet static  
address 192.168.1.1/27  
  
auto ens224:1  
iface ens224:1 inet static  
address 192.168.2.1/28    

auto ens224.100  
iface ens224.100 inet static  
address 192.168.1.3/27    
Vlan-raw-device ens224  
  
auto ens224.200  
iface ens224.200 inet static  
address 192.168.2.3/28    
Vlan-raw-device ens224:1

auto ens224.99
iface ens224.99 inet static
address 192.168.99.1/29
```

```
systemctl restart networking
```

</details>

<a id="ssh"></a>
## 6. Настройка SSH

<details>
<summary>Задание</summary>

Настройте безопасный удаленный доступ на серверах HQ-SRV и BR-SRV:

- Для подключения используйте порт 2026
- Разрешите подключения исключительно пользователю sshuser
- Ограничьте количество попыток входа до двух
- Настройте баннер «Authorized access only».
</details>

### HQ-SRV и BR-SRV

Устанавливаем ssh

```
apt install openssh-server -y
```

Открываем конфиг

```
nano /etc/ssh/sshd_config
```
Добавляем конфигурацию

```
Port 2026
MaxAuthTries 2
PasswordAuthentication yes
Banner /etc/ssh/bannermotd
AllowUsers sshuser
```

Далле нужно создать файл для баннера 

```
nano /etc/ssh/bannermotd
````

Добавляем в него текст приветствия

```
----------------------
Authorized access only
----------------------
```

Перезапускаем SSH 

```
systemctl restart sshd
```

</details>

## 7 Настройка GRE-туннеля

<details>
<summary>Задание</summary>
Между офисами HQ и BR, на маршрутизаторах HQ-RTR и BR-RTR необходимо сконфигурировать ip туннель:
  
- На выбор технологии GRE или IP in IP
- Сведения о туннеле занесите в отчёт.
</details>

<details>
<summary>Решение</summary>
    
### HQ-RTR

Переходим в конфиг интерфейсов

```
nano /etc/network/interfaces
```

Прописываем интерфейс gre тунеля

```
auto gre1
iface gre1 inet tunnel
address 10.10.10.1
netmask 255.255.255.252
mode gre
local 172.16.1.2
endpoint 172.16.2.2
ttl 64
```

Добавляем модуль gre

```
nano /etc/modules
```

Добавляем

```
gre_ip
```

<img width="864" height="633" alt="image" src="https://github.com/user-attachments/assets/8e5ec0ac-ec7f-4044-9f35-759ea57d47e0" />


Перезагружаем

```
systemctl restart networking
```

### BR-RTR

Переходим в конфиг интерфейсов

```
nano /etc/network/interfaces
```

Прописываем интерфейс gre тунеля

```
auto gre1
iface gre1 inet tunnel
address 10.10.10.2
netmask 255.255.255.252
mode gre
local 172.16.2.2
endpoint 172.16.1.2
ttl 64
```

Добавляем модуль gre

```
nano /etc/modules
```

Добавляем

```
gre_ip
```

<img width="805" height="586" alt="image" src="https://github.com/user-attachments/assets/64843e81-052b-443c-a04a-945722f2e893" />

Перезагружаем

```
systemctl restart networking
```

### Провекрка 

На всех машинах

```
ip -c a
```

HQ-RTR

```
ping 10.10.10.2
```

<img width="817" height="273" alt="image" src="https://github.com/user-attachments/assets/e5dda0cf-8192-4c68-a6a8-daf49b50441a" />


BR-RTR

```
ping 10.10.10.1
```

<img width="917" height="269" alt="image" src="https://github.com/user-attachments/assets/67bf81b9-da67-41ef-9cb4-d853a63744e3" />

</details>

