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

ISP
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

HQ-RTR
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

BR-RTR
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

