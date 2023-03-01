# Лабораторная работа - Развертывание коммутируемой сети с резервными каналами

**Выполнил Давыдкин Д.Г.**

------------
### Топология

![Image]()


### Таблица адресации

|  Устройство | Интерфейс  |IP-адрес   |Маска подсети   |
|---|---|---|---|
| S1 | VLAN 1  | 192.168.1.1  | 255.255.255.0  |
|S2   | VLAN 1  |  192.168.1.2 | 255.255.255.0  |
|  S3 | VLAN 1  | 192.168.1.3  | 255.255.255.0  |


### Задачи
Часть 1. Создание сети и настройка основных параметров устройства

Часть 2. Выбор корневого моста

Часть 3. Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов 

Часть 4. Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов




### Часть 1:	Создание сети и настройка основных параметров устройства

*Шаг 1. Соберите сеть согласно топологии. Шаг 2:	Выполните инициализацию и перезагрузку коммутаторов.*

![Image]()

*Шаг 2:	Выполните инициализацию и перезагрузку коммутаторов. *

*Шаг 3:	Настройте базовые параметры каждого коммутатора. *

Switch>en

Switch#clock set 22:00:00 feb 01 2023

Switch#conf t

Switch(config)#hostname S1

S1(config)#no ip domain-lookup

S1(config)#enable secret class

S1(config)#service password-encryption

S1(config)#banner motd #Entry closed!!!#

S1(config)#line con 0

S1(config-line)#logging synchronous

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#line vty 0 4

S1(config-line)#logging synchronous

S1(config-line)#password cisco

S1(config-line)#login

S1(config-line)#int vlan 1

S1(config-if)#ip address 192.168.1.1 255.255.255.0

S1(config-if)#no shu

S1(config-line)#do wr


Switch>en

Switch#clock set 22:00:00 feb 01 2023

Switch#conf t

Switch(config)#hostname S2

S2(config)#no ip domain-lookup

S2(config)#enable secret class

S2(config)#service password-encryption

S2(config)#banner motd #Entry closed!!!#

S2(config)#line con 0

S2(config-line)#logging synchronous

S2config-line)#password cisco

S2(config-line)#login

S2(config-line)#line vty 0 4

S2(config-line)#logging synchronous

S2(config-line)#password cisco

S2(config-line)#login

S2(config-line)#int vlan 1

S2(config-if)#ip address 192.168.1.2 255.255.255.0

S2(config-if)#no shu

S2(config-line)#do wr

Switch>en

Switch#clock set 22:00:00 feb 01 2023

Switch#conf t

Switch(config)#hostname S3

S3(config)#no ip domain-lookup

S3(config)#enable secret class

S3(config)#service password-encryption

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

S3(config-if)#ip address 192.168.1.3 255.255.255.0

S3(config-if)#no shu

S3(config-line)#do wr


*Шаг 4:	Проверьте связь.*

**Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S2?**

Да

![Image]()

**Успешно ли выполняется эхо-запрос от коммутатора S1 на коммутатор S3?**

Да

![Image]()

**Успешно ли выполняется эхо-запрос от коммутатора S2 на коммутатор S3?**

Да

![Image]()


### Часть 2:	Определение корневого моста

*Шаг 1:	Отключите все порты на коммутаторах.*

S1(config)#int range eth0/0-3

S1(config)#shu

S2(config)#int range eth0/0-3

S2(config)#shu

S3(config)#int range eth0/0-3

S3(config-if-range)#shu


*Шаг 2:	Настройте подключенные порты в качестве транковых.*

S1(config-if-range)#switchport trunk encapsulation dot1Q

S1(config-if-range)#switchport mode trunk


S2(config-if-range)#switchport trunk encapsulation dot1Q

S2(config-if-range)#switchport mode trunk


S3(config-if-range)#switchport trunk encapsulation dot1Q

S3(config-if-range)#switchport mode trunk

*Шаг 3:	Включите порты F0/2 и F0/4 на всех коммутаторах.*

S1(config)#int range eth0/0,eth0/3

S1(config-if-range)#no shu


S2(config)#int range eth0/0,eth0/3

S2(config-if-range)#no shu


S3(config)#int range eth0/0,eth0/3

S3(config-if-range)#no shu

*Шаг 4:	Отобразите данные протокола spanning-tree.*

show spanning-tree S1

![Image]()


show spanning-tree S2

![Image]()


show spanning-tree S3

![Image]()


**С учетом выходных данных, поступающих с коммутаторов, ответьте на следующие вопросы.
Какой коммутатор является корневым мостом?**

Ответ: Коммутатор S1.

**Почему этот коммутатор был выбран протоколом spanning-tree в качестве корневого моста?**

Ответ: Из-за наименьшего значения MAC-адреса.

**Какие порты на коммутаторе являются корневыми портами?** 

Ответ: eth0/1

**Какие порты на коммутаторе являются назначенными портами?**

Ответ: eth0/3

**Какой порт отображается в качестве альтернативного и в настоящее время заблокирован?** 

Ответ: Такие порты отсутствуют, так как у нас сейчас по одному включенному линку к каждому коммутатору. 

**Почему протокол spanning-tree выбрал этот порт в качестве невыделенного (заблокированного) порта?** 

Ответ: Такие порты отсутствуют.


### Часть 3:	Наблюдение за процессом выбора протоколом STP порта, исходя из стоимости портов

Перед началом выполнения 3-ей части включил все порты.

*Шаг 1:	Определите коммутатор с заблокированным портом.*

Ответ:Коммутаторы S2 и S3. Мною был выбран коммутатор S2.

![Image]()



*Шаг 2:	Измените стоимость порта.*

S2(config)#int eth0/0

S2(config-if)#spanning-tree cost 10

*Шаг 3:	Просмотрите изменения протокола spanning-tree.*

![Image]()

*Шаг 4:	Удалите изменения стоимости порта.*



**Почему протокол spanning-tree заменяет ранее заблокированный порт на назначенный порт и блокирует порт, который был назначенным портом на другом коммутаторе?**

Ответ: 1) Так как уменьшяется стоимость прохождения через данный порт. 2) Блокирует порт по причине перерасчета стоимости путей всего дерева протокола Spannig-tree.

### Часть 4:	Наблюдение за процессом выбора протоколом STP порта, исходя из приоритета портов

*a.	Включите порты F0/1 и F0/3 на всех коммутаторах.

Было включено ранее, так как отсутствовали заблокированные порты.

*b.	Подождите 30 секунд, чтобы протокол STP завершил процесс перевода порта, после чего выполните команду show spanning-tree на коммутаторах некорневого моста. *

![Image]()

![Image]()

![Image]()

### Вопросы для повторения

**1.	Какое значение протокол STP использует первым после выбора корневого моста, чтобы определить выбор порта?**

Ответ: Стоимость пути до корневого моста.

**2.	Если первое значение на двух портах одинаково, какое следующее значение будет использовать протокол STP при выборе порта?**

Ответ: Значение BridgeID.

**3.	Если оба значения на двух портах равны, каким будет следующее значение, которое использует протокол STP при выборе порта?** 

Ответ:  Значение PortID.
