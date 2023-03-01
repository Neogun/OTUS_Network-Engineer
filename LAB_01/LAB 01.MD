# Лабораторная работа -  

**Выполнил Давыдкин Д.Г.**

------------
### Топология

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/Topology.jpg)


### Таблица адресации

|  Устройство | Интерфейс  | IP-адрес  | Маска| Шлюз по умолчанию  |
| ------------ | ------------ | ------------ | ------------ | ------------ |
| R1 | eth0/0.3 | 192.168.3.1  | 255.255.255.0 |-   |
|  | eth0/0..4  | 192.168.4.1  | 255.255.255.0  |-   |
|  | G0/0/0 G0/0/1  | -  | -  |-   |
|S1 | VLAN 3  |192.168.3.11 |255.255.255.0  | 192.168.3.1 |
|S2 | VLAN 3  | 192.168.3.12  |255.255.255.0   | 192.168.3.1  |
| PC-A  | NIC  | 192.168.3.3 |255.255.255.0 |192.168.3.1  |
| PC-B  | NIC  | 192.168.4.3 | 255.255.255.0 | 192.168.4.1|

### Таблица VLAN

| VLAN  | Имя VLAN  |  Назначенный интерфейс |
|---|---|---|
| 3  | Management  | S1: VLAN 3 S2: VLAN 3 S1: eth0/2  |
|4  | Operations| S2: eth0/2  |
|  7 |  ParkingLot | S1: eth0/3 S2: eth0/3  |
|8   | Native  | -  |

### Задачи
Часть 1. Построить сеть и произвести базовые настройки оборудования.  

Часть 2. Созать VLANы и назначить порты.  

Часть 3. Настроить транки 802.1Q между коммутаторами.  

Часть 4. Настроить межвлановую маршрутизацию на роутере. 

Часть 5. Проверить работы межвлановой маршрутизации.  


### Часть 1:	Создание сети и настройка основных параметров устройства

*Шаг 1.Создайте сеть согласно топологии. Шаг 2:	Выполните инициализацию и перезагрузку коммутаторов. *

![Image]()


*Шаг 3. Настройте каждый коммутатор коммутатор. *

Switch>en

Switch#clock set 22:00:00 feb 01 2023

Switch#conf t

Switch(config)#hostname S1

S1(config)#no ip domain-lookup

S1(config)#enable secret class

S1(config) service password-encryption

S1(config)#banner motd #Entry closed!!!#

S1(config)#line con 0

S1(config-line)#logging synchronous

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#line vty 0 4

S1config-line)#logging synchronous

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#int vlan 1

S1(config-if)#ip address 192.168.1.2 255.255.255.0

S1(config-if)#no shu

S1(config-if)#do wr


Switch>en

Switch#clock set 22:00:00 feb 01 2023

Switch#conf t

Switch(config)#hostname S2

S2(config)#no ip domain-lookup

S2(config)#enable secret class

S2(config) service password-encryption

S2(config)#banner motd #Entry closed!!!#

S2(config)#line con 0

S2(config-line)#logging synchronous

S2(config-line)#password cisco

S2(config-line)#login

S2(config-line)#line vty 0 4

S2(config-line)#logging synchronous

S2(config-line)#password cisco

S2(config-line)#login

S2(config-line)#int vlan 1

S2(config-if)#ip address 192.168.1.2 255.255.255.0

S2(config-if)#no shu

S2(config-if)#do wr



Switch>en

Switch#clock set 22:00:00 feb 01 2023

Switch#conf t

Switch(config)#hostname S3

S3(config)#no ip domain-lookup

S3(config)#enable secret class

S3(config) service password-encryption

S3(config)#banner motd #Entry closed!!!#

S3(config)#line con 0

S3(config-line)#logging synchronous

S3(config-line)#password cisco

S3(config-line)#login

S3(config-line)#line vty 0 4

S3(config-line)#logging synchronous

S3(config-line)#password cisco

S3(config-line)#login

S3(config-line)#int vlan 1

S3(config-if)#ip address 192.168.1.2 255.255.255.0

S3(config-if)#no shu

S3(config-if)#do wr



*Шаг 4. Настройте адресацию на PC-A и PC-B *

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/PC-A%20IP-address.jpg)

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/PC-B%20IP-address%20.jpg)


### Часть 2. Созать VLANы и назначить порты.

*Шаг 1. Создайте требуемые VLAN на обоих коммутаторах из таблицы выше *


S1(config)#vlan 3

S1(config-vlan)#name Management

S1(config-vlan)#vlan 4

S1(config-vlan)#name Operations

S1(config-vlan)#vlan 7

S1(config-vlan)#name ParkingLot

S1(config-vlan)#vlan 8

S1(config-vlan)#name Native


S2(config)#vlan 3

S2(config-vlan)#name Management

S2(config-vlan)#vlan 4

S2(config-vlan)#name Operations

S2(config-vlan)#vlan 7

S2(config-vlan)#name ParkingLot

S2(config-vlan)#vlan 8

S2(config-vlan)#name Native

*Шаг 2. Создайте интерфейс управления и назначьте маршрут по умолчанию для каждого коммутатора из таблицы адресации *

S1(config) ip default-gateway 192.168.3.1

S1(config)#int vlan 3

S1(config-if)#no shu

S1(config-if)#ip address 192.168.3.11 255.255.255.0


S2(config) ip default-gateway 192.168.3.1

S2(config)#int vlan 3

S2(config-if)#no shu

S2(config-if)#ip address 192.168.3.12 255.255.255.0

*Шаг 3. Назначьте все неиспользуемые порты на обоих коммутаторах в VLAN ParkingLot и административно выключите их переведите в режим доступа *

S1(config)#int eth0/3

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan 7

S1(config-if)#shu



S2(config)#int eth0/3

S2(config-if)#switchport mode access

S2(config-if)#switchport access vlan 7

S2(config-if)#shu


*Шаг 4. Назначьте все используемые порты доступа на обоих коммутаторах в соответствующий VLAN , согласно таблицы VLAN и переведите их в режим доступа*

S1(config)#int eth0/2

S1(config-if)#switchport mode access

S1(config-if)#switchport access vlan3


S2(config)#int eth0/2

S2(config-if)#switchport mode access

S2(config-if)#switchport access vlan 4


*Шаг 5. Убедитесь, что vlan назначены корректно*

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/show%20vlan%20brief%20S1.jpg)

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/show%20vlan%20brief%20S2.jpg)

### Часть 3. Конфигурация Транка 802.1Q между коммутаторами.

*Шаг 1. Переведите интерфейс eth0/1 в режим транка на обоих коммутаторах*

S1(config)#int eth0/1

S1
(config-if)#switchport trunk encapsulation dot1Q 

S1(config-if)#switchport mode trunk


S2(config)#int eth0/1

S2(config-if)#switchport trunk encapsulation dot1Q 

S2(config-if)#switchport mode trunk


*Шаг 2. Сделайте VLAN 8 нативным на обоих коммутаторах*

S1(config-if)#switchport trunk native vlan 8

S2(config-if)#switchport trunk native vlan 8

*Шаг 3. Настройте передачу трафика VLANов 3,4,8 для передачи через транковый порт*

S1(config-if)#switchport trunk allowed vlan 3,4,8

S2(config-if)#switchport trunk allowed vlan 3,4,8

*Шаг 4. Проверьте настройку транковых портов и разрешенных для передачи через транк VLANов*

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/show%20int%20trunk%20S1.jpg)

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/show%20int%20trunk%20S2.jpg)

*Шаг 5. Настройте интерфейс коммутатора S1 eth0/0 в режим транка*

S1(config)#int eth0/1

S1(config-if)#switchport trunk encapsulation dot1Q

S1(config-if)#switchport mode trunk

S1(config-if)#switchport trunk native vlan 8

S1(config-if)#switchport trunk allowed vlan 3,4,8

S1(config-if)#do wr

 *Шаг 6. Проверьте настройку транковых портов и разрешенных для передачи через транк VLANов на коммутаторе S1 еще раз*

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/show%20int%20trunk%20S1.jpg)


### Часть 4. Настроить межвлановую маршрутизацию на маршрутизаторе.   

*Шаг 1. Включите интерфейс eth0/0 на маршрутизаторе*

R1(config)#int eth0/0

R1(config-if)#no shu

*Шаг 2. Настройте саб-интерфейсы для  VLAN 3 и 4, с IP-адресами указанными в таблице. Все саб-интерфейсы должны использовать инкапсуляцию 802.1Q. Настройте описание для каждого саб-интерфейса. Саб-интерфейс для нативного VLAN не имеет IP-адреса*

R1(config)#int eth0/0.3

R1(config-if)#ip address 192.168.3.1 255.255.255.0

R1(config-if)#encapsulation dot1Q 3

R1(config-if)#description Management

R1(config-if)#int eth0/0.4

R1(config-if)#ip address 192.168.4.1 255.255.255.0

R1(config-if)#encapsulation dot1Q 4

R1(config-if)#description Operations

### Часть 5. Проверить работы межвлановой маршрутизации. 

*Шаг 1. Ввыполните команду ping с PC-A до шлюза по умолчанию*

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/PC-A%20ping%20default%20gateway.jpg)

*Шаг 2. Ввыполните команду ping с PC-A до PC-B*

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/PC-A%20ping%20PC-B.jpg)

*Шаг 3. Ввыполните команду ping с PC-A до S2*

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/PC-A%20ping%20S2.jpg)

*Шаг 4. Выполните команду tracert с PC-B до PC-A*

![Image](https://github.com/Neogun/OTUS_Network-Engineer/blob/main/LAB_01/Images/PC-B%20tracert%20to%20PC-A.jpg)

 **Какой промежуточный IP-адрес отображен в результате команды Tracert?** Промежуточный IP-адрес указан 192.168.4.1, который является шлюзом по умолчанию для PC-B.
 
 
