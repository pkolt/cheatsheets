# Linux

- [Linux](#linux)
  - [Утилита apt в Debian](#утилита-apt-в-debian)
  - [Просмотр информации](#просмотр-информации)
  - [Выключение, запуск программ](#выключение-запуск-программ)
  - [Установка шрифтов Microsoft](#установка-шрифтов-microsoft)
  - [Установка Debian 10 (из Mac OS)](#установка-debian-10-из-mac-os)
  - [Настройка](#настройка)
  - [Графическая оболочка](#графическая-оболочка)
  - [Пользователи](#пользователи)
  - [Сеть](#сеть)
    - [7 уровней модели OSI](#7-уровней-модели-osi)
    - [Утилита `ip`](#утилита-ip)
    - [Ping](#ping)
    - [Настройка обращения к DNS](#настройка-обращения-к-dns)
    - [nftables](#nftables)
      - [Архитектура nftables](#архитектура-nftables)
      - [Работа с сервисом nftables](#работа-с-сервисом-nftables)
      - [Базовый конфиг](#базовый-конфиг)
      - [Утилита nft](#утилита-nft)
    - [Утилита traceroute](#утилита-traceroute)
    - [Утилита tcpdump](#утилита-tcpdump)
    - [Утилита iftop](#утилита-iftop)
    - [Утилита nethogs](#утилита-nethogs)
    - [Работа с SSH](#работа-с-ssh)
    - [Использование ssh-agent](#использование-ssh-agent)
      - [Добавление alias в SSH](#добавление-alias-в-ssh)
      - [Выключить вход по паролю в SSH](#выключить-вход-по-паролю-в-ssh)
    - [UFW (Uncomplicated Firewall)](#ufw-uncomplicated-firewall)
    - [DNS](#dns)
    - [Узнать какие порты открыты](#узнать-какие-порты-открыты)
    - [Перевод программы в фоновый режим и возврат к ней (Screen)](#перевод-программы-в-фоновый-режим-и-возврат-к-ней-screen)
    - [Dropbear (подключение по SSH до монтирования диска)](#dropbear-подключение-по-ssh-до-монтирования-диска)
    - [VNC](#vnc)
    - [Отключить Wi-Fi](#отключить-wi-fi)
  - [Файлы и каталоги](#файлы-и-каталоги)
    - [Форматирование флешки](#форматирование-флешки)
    - [Работа с диском](#работа-с-диском)
    - [Проверка целостности файла](#проверка-целостности-файла)
  - [Шифрование](#шифрование)
    - [GPG (GNU Privacy Guard)](#gpg-gnu-privacy-guard)
  - [Настройка подписи коммитов в GIT с помощью GPG](#настройка-подписи-коммитов-в-git-с-помощью-gpg)
    - [Zip (MacOS)](#zip-macos)
    - [Посмотреть температуру дисков и процессора](#посмотреть-температуру-дисков-и-процессора)
  - [Загрузки](#загрузки)
    - [yt-dlp](#yt-dlp)

## Утилита apt в Debian

Основной инструмент управления пакетами в Debian.  
Она используется для установки, обновления, удаления и поиска программ.  
Под капотом вызывает команду `dpkg`.

```bash
# Обновляет информацию о доступных пакетах из репозиториев
sudo apt update

# Обновляет все установленные пакеты до последних версий
sudo apt upgrade

# Может удалять или устанавливать пакеты для разрешения зависимостей
sudo apt full-upgrade

# Установка пакета (пакетов)
sudo apt install <имя_пакета>

# Удаление пакета
sudo apt remove <имя_пакета>

# Полное удаление (включая конфиги)
sudo apt purge <имя_пакета>

# Удаляет ненужные зависимости
sudo apt autoremove

# Очищает кэш скачанных пакетов (/var/cache/apt/archives)
sudo apt clean

# Поиск пакетов
apt search <имя_пакета>

# Информация о пакете
apt show <имя_пакета>

# APT использует список репозиториев из файла:
cat /etc/apt/sources.list
# и каталога:
ls -l /etc/apt/sources.list.d/

# Установка конкретной версии
sudo apt install nginx=1.22.1

# Показ установленных пакетов
apt list --installed

# Проверка обновлений
apt list --upgradable

# Фиксация версии (hold) (говорит системе - не трогай этот пакет, даже если есть новая версия)
sudo apt-mark hold nginx
# Снять фиксацию (unhold)
sudo apt-mark unhold nginx
```

## Просмотр информации

* `free -m` количество оперативной памяти
* `df -h` количество места на жестком диске
* `lscpu` архитектура процессора
* `uptime` нагрузка системы за 1, 5, 15 минут
* `ps -u <пользователь> fu` процессы пользователя в виде дерева
* `which <программа>` узнать каталог в котором физически находится исполняемый файл программы
* `hexdump -C <файл>` побайтовый просмотр файла

## Выключение, запуск программ

* `shutdown -h now` выключение системы
* `restart` перезагрузка

## Установка шрифтов Microsoft

```bash
sudo rm -rf /var/lib/update-notifier/package-data-downloads/partial/*
sudo apt-get --purge --reinstall install ttf-mscorefonts-installer
sudo fc-cache -fv
```

## Установка Debian 10 (из Mac OS)

1. Определить имя для диска с флешкой

```bash
diskutil list
```

2. Конвертировать ISO файл в образ диска

```bash
hdiutil convert -format UDRW -o debian-10.5.0-amd64-netinst.img debian-10.5.0-amd64-netinst.iso
```

3. Переименовать файл образа из *.IMG.DMG в *.IMG

```bash
mv debian-10.5.0-amd64-netinst.img.dmg debian-10.5.0-amd64-netinst.img
```

4. Размонтировать диск с флешкой перед записью образа Debian

```bash
sudo diskutil unmountDisk /dev/disk2
```

5. Запись образа Debian на диск

```bash
sudo dd if=debian-10.5.0-amd64-netinst.img of=/dev/disk2 bs=16M status=progress oflag=sync
```

## Настройка

* `sudo dpkg-reconfigure tzdata` смена временной зоны
* `sudo update-alternatives --config editor` задать текстовый редактор по умолчанию
* `sudo apt-get install gnome-tweak-tool` вернуть минимизацию окна, "Заголовки окон" -> "Кнопки заголовка окна"

## Графическая оболочка

* `systemctl get-default` текущее состояние
* `systemctl set-default multi-user.target` загрузка Linux без графической оболочки
* `systemctl set-default graphical.target` загрузка Linux с графической оболочкой

## Пользователи

* `adduser <имя>` добавить пользователя
* `visudo` разрешить запуск команд суперпользователя из под sudo

## Сеть

### 7 уровней модели OSI

| Уровень                         | Описание                                | Протоколы                                    | Утилиты (Linux)                   |
|---------------------------------|-----------------------------------------|----------------------------------------------|-----------------------------------|
| 7. Прикладной (Application)     | Пользовательские приложения и протоколы | HTTP, HTTPS, FTP, SSH, DNS, SMTP, IMAP, POP3 | curl, wget, ssh, ftp, dig, host   |
| 6. Представления (Presentation) | Формат данных, шифрование, кодирование  | TLS/SSL, JPEG, MPEG, ASCII, UTF-8            | openssl, base64                   |
| 5. Сеансовый (Session)          | Управление сессиями                     | NetBIOS, RPC, SIP, PPTP                      | ssh, tmux, screen                 |
| 4. Транспортный (Transport)     | Сквозная передача (TCP/UDP), порты      | TCP, UDP, SCTP                               | ss, netstat, nc (netcat), lsof -i |
| 3. Сетевой (Network)            | IP-адресация и маршрутизация            | IP (IPv4, IPv6), ICMP, IPsec                 | ip, ping, traceroute              |
| 2. Канальный (Data Link)        | MAC-адреса, ARP, кадры                  | Ethernet, ARP, PPP, VLAN                     | ip link, arp, arping, tcpdump     |
| 1. Физический (Physical)        | Физическая передача (кабели, сигнал)    | Wi-Fi, DSL, Bluetooth, USB                   | ethtool, iw, dmesg                |

### Утилита `ip`

Утилита `ip` из пакета `iproute2` пришла на смену устаревшему пакету `net-tools` (содержащему команды `ifconfig`, `route`, `arp` и другие)

1. `ip link` - управление интерфейсами

```bash
# Показывает состояние всех интерфейсов
ip link show
# или сокращённо
ip l

# Поднять (включить) интерфейс
ip link set eth0 up
# Опустить (выключить) интерфейс
ip link set eth0 down
# Изменить MAC-адрес
ip link set eth0 address 00:11:22:33:44:55
# Установить MTU:
ip link set eth0 mtu 1500

# Добавить виртуальный интерфейс (например, bridge):
ip link add name br0 type bridge
ip link set br0 up

# Удалить интерфейс
ip link delete br0
```

2. `ip addr` - управление IP-адресами

```bash
# Показать все IP-адреса на всех интерфейсах
ip addr show
# или сокращённо
ip a
# Показать адреса только на конкретном интерфейсе
ip addr show eth0

# Добавить IP-адрес (⚠️ не добавлять на ПК несколько IP из одного сегмента)
ip addr add 192.168.1.10/24 dev eth0
# Добавить вторичный адрес (alias)
ip addr add 192.168.1.11/24 dev eth0 label eth0:1
# Удалить адрес
ip addr del 192.168.1.10/24 dev eth0
# Очистить все адреса на интерфейсе
ip addr flush dev eth0
```

3. `ip route` - управление маршрутами

```bash
# Показать таблицу маршрутизации
ip route show
# или сокращённо
ip r

# Показать маршруты для определённой сети/адреса
ip route get 8.8.8.8
# Добавить маршрут по умолчанию (шлюз)
ip route add default via 192.168.1.1
# Добавить маршрут к подсети через шлюз
ip route add 10.0.0.0/8 via 192.168.1.1 dev eth0
# Добавить маршрут к подсети через интерфейс (без шлюза, прямая сеть)
ip route add 10.0.0.0/8 dev eth0
# Удалить маршрут
ip route del default via 192.168.1.1
ip route del 10.0.0.0/8
# Использовать несколько таблиц маршрутизации (с таблицей 100)
ip route add 192.168.100.0/24 dev eth1 table 100
```

- `via` — IP-адрес следующего маршрутизатора (шлюза);
- `dev` — сетевой интерфейс, через который уходит трафик;
- `proto` — источник добавления маршрута: `kernel` (ядро), `dhcp` (DHCP-клиент), `static` (ручное добавление) и т.д.;
- `src` — предпочитаемый исходный IP-адрес для исходящих пакетов;
- `metric` — числовой приоритет (меньше = лучше);
- `scope` — область действия: `global` (маршрут к удалённой сети), `link` (прямое подключение), `host` (адрес самого хоста);
- `linkdown` — флаг, означающий, что интерфейс не в состоянии `UP` (не работает).

4. `ip neigh` - ARP/NDP (соседи)

```bash
# Показать ARP-таблицу (IPv4) и ND-таблицу (IPv6)
ip neigh show
# Удалить запись
ip neigh del 192.168.1.5 dev eth0
# Добавить статическую ARP-запись
ip neigh add 192.168.1.100 lladdr 00:1a:2b:3c:4d:5e dev eth0 nud permanent
```

5. `ip netns` - сетевые пространства имён (контейнеры, изоляция)

```bash
# Показать все пространства:
ip netns list
# Создать пространство имён
ip netns add myns
# Выполнить команду внутри пространства
ip netns exec myns ip addr show
# Запустить оболочку внутри пространства:
ip netns exec myns bash
# Удалить пространство
ip netns delete myns
```

### Ping

Посылает маленький ICMP-пакет (56 или 64 байта) и ждет ответ.

```bash
# Базовая проверка связи
ping <адрес>
# Отправить ограниченное число пакетов
ping -c <число> <адрес>
# Задать интервал между пакетами (сек)
ping -i <сек> <адрес>
# Непрерывный пинг с метками времени
ping -D <адрес>
# Флуд‑режим (только root)
sudo ping -f <адрес>
# Остановить через заданное время (сек)
ping -w <сек> <адрес>
# Использовать конкретный сетевой интерфейс
ping -I <интерфейс> <адрес>
# Звуковой сигнал при ответе
ping -a <адрес>
# Принудительно IPv4 или IPv6
ping -4 <адрес>
ping -6 <адрес>
```

### Настройка обращения к DNS

```bash
# Управляет тем, как система разрешает имена хостов
cat /etc/host.conf

# Локальная таблица соответствия имён хостов и IP-адресов
cat /etc/hosts

# Конфигурация DNS-резолвера
cat /etc/resolv.conf
```

### nftables

[Документация по nftables](http://wiki.nftables.org)

Это firewall + система обработки пакетов:

- фильтрация трафика (разрешить/запретить)
- NAT (маскарадинг, проброс портов)
- логирование
- защита от атак (например, rate limiting)

#### Архитектура nftables

В nftables структура выглядит так:

```
table → chain → rule
```

1. **Table (таблица). Контейнер для правил.**

Типы:

`inet` — IPv4 + IPv6 (чаще всего используешь)
`ip` — только IPv4
`ip6` — только IPv6
`bridge`, `arp`

2. **Chain (цепочка). Где именно применяются правила.**

Основные:

- `input` — входящий трафик
- `output` — исходящий
- `forward` — транзитный

Есть параметры:

- `type filter`
- `hook input`
- `priority 0`

3. Rule (правило).

Конкретное условие:

```
ip saddr 192.168.1.1 accept
tcp dport 22 drop
```

#### Работа с сервисом nftables

```bash
# Посмотреть статус
sudo systemctl status nftables.service

# Включить автозагрузку сервиса
sudo systemctl enable nftables.service

# Запустить сервис сейчас
sudo systemctl start nftables.service
```

#### Базовый конфиг

Базовый конфиг в файле `/etc/nftables.conf`

```
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
	chain input {
		type filter hook input priority filter;
	}
	chain forward {
		type filter hook forward priority filter;
	}
	chain output {
		type filter hook output priority filter;
	}
}
```

#### Утилита nft

```bash
# Показывает все таблицы, которые сейчас существуют
sudo nft list tables
# Вывод: table inet filter (таблица для фильтрации (IPv4 + IPv6))

# Показать одну таблицу
sudo nft list table inet filter

# Показывает всё полностью (таблицы, цепочки, правила, sets / maps)
# ⚠️ Показывает не файл /etc/nftables.conf а то, что реально загружено в ядре. Они могут отличаться!
sudo nft list ruleset

# Показывает все цепочки
sudo nft list chains
# Только правила входящего трафика
sudo nft list chain inet filter input

# Применить правила
sudo nft -f /etc/nftables.conf
```

### Утилита traceroute

Это утилита для диагностики сети, которая показывает путь пакетов от твоего компьютера до целевого хоста (по каким маршрутизаторам они проходят).

Идея основана на поле **TTL (Time To Live)** в IP-пакетах:

1. Отправляется пакет с TTL=1
→ первый роутер уменьшает TTL до 0 и возвращает ошибку
2. Затем с TTL=2
→ доходит до второго роутера
3. И так далее…

В итоге ты получаешь список всех "хопов" (узлов) по пути.

```bash
traceroute google.com

# Использовать ICMP (как ping)
traceroute -I google.com

# Использовать TCP (имитирует обычное соединение, например - HTTP)
traceroute -T google.com

# Указать порт
traceroute -T -p 443 google.com

# Ограничить число хопов
traceroute -m 10 google.com

# Быстрее (меньше запросов)
traceroute -q 1 google.com
```

Почему появляются `* * *` ?

- роутер игнорирует ICMP (часто на VPN или у провайдеров)
- firewall блокирует ответы
- пакет потерялся

Это не всегда проблема, если дальше маршрут продолжается.

**Альтернатива (лучше traceroute)**

```bash
sudo apt install mtr
mtr google.com
```

### Утилита tcpdump

```bash
# Покажет ВСЕ пакеты на интерфейсе по умолчанию.
sudo tcpdump

# Покажет ВСЕ пакеты на интерфейсе eth0 (см. интерфейсы `ip a`)
sudo tcpdump -i eth0

# Слушать все интерфейсы
sudo tcpdump -i any

# Без DNS-резолва (иначе tcpdump будет тормозить, пытаясь резолвить IP → домены)
sudo tcpdump -n

# Показать MAC-адреса
sudo tcpdump -e

# Ограничить количество пакетов
sudo tcpdump -c 10

# Фильтрация по IP
sudo tcpdump host 8.8.8.8

# Только входящий траффик
sudo tcpdump dst host 192.168.1.10

# Только исходящий траффик
sudo tcpdump src host 192.168.1.10

# По порту (весь траффик где встречается порт)
sudo tcpdump port 80
sudo tcpdump tcp port 22     # SSH
sudo tcpdump udp port 53     # DNS

# Комбинации
sudo tcpdump host 1.1.1.1 and port 443
sudo tcpdump port 80 or port 443
sudo tcpdump not port 22

# Показать содержимое пакетов (HEX + ASCII)
sudo tcpdump -X
# (только ASCII)
sudo tcpdump -A

# Подробный вывод
sudo tcpdump -v
sudo tcpdump -vv
sudo tcpdump -vvv

# Не обрезать пакеты
sudo tcpdump -s 0

# Сохранение в файл (для просмотра в Wireshark)
sudo tcpdump -w dump.pcap

# Чтение из файла
tcpdump -r dump.pcap
```

### Утилита iftop

Используется для мониторинга сетевого трафика в реальном времени в Linux.

```bash
# Установка
sudo apt update
sudo apt install iftop

# Запуск
sudo iftop

# Указать интерфейс
sudo iftop -i eth0
```

Основные горячие клавиши (прямо во время работы):

| Клавиша | Что делает               |
|---------|--------------------------|
| h       | помощь                   |
| t       | смена режима отображения |
| n       | отключить DNS (важно!)   |
| p       | показать порты           |
| P       | скрыть порты             |
| b       | показать bar-графики     |
| s       | только исходящий         |
| d       | только входящий          |
| q       | выход                    |

### Утилита nethogs

Позволяет понять какой именно процесс использует сеть.

```bash
# Установка
sudo apt update
sudo apt install nethogs

# Запуск (все сетевые интерфейсы)
sudo nethogs
# Указанный интерфейс (или несколько)
sudo nethogs eth0

# Показ командной строки процесса
sudo nethogs -l

# Только TCP/UDP
sudo nethogs -C

# Только конкретный процесс (где 1234 — PID)
sudo nethogs -P 1234

# Управление во время работы
# q — выйти
# s — сортировка по отправке
# r — по приёму
# m — смена единиц (KB/s, MB/s и т.д.)
```

### Работа с SSH

* `ssh-keygen -t ed25519 -C <comment> -f <private_key>` создание публичного и приватного ключей (`ssh-keygen -t ed25519 -C alice_github -f ~/.ssh/id_github_ed25519`)
* `ssh-keygen -c -C <comment> -f <private_key>` изменить комментарий ключа
* `pbcopy < ~/.ssh/id_ed25519.pub` копирование публичного ключа в буфер обмена
* `ssh-copy-id -i ~/.ssh/id_rsa.pub username@host` передача открытого ключа на другой компьютер
* `ssh -L 8000:192.168.0.100:80 user@host` переброс удаленного порта на локальный (например получить доступ к локальному сайту)
* `ssh -i <private_key.txt> username@host` подключение через приватный ключ
* `ssh-keygen -R <IP>` при ошибке - `WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!`

### Использование ssh-agent

Настройка:

```
# ~/.ssh/config
Host example.com
  ForwardAgent yes
```

* `ssh-add <private_key>` добавление ключа
* `ssh-add --apple-use-keychain <private_key>` добавление ключа (запомнить ключи, даже после перезапуска ПК в macOS)
* `ssh-add -l` или `ssh-add -L` показать список ключей
* `ssh-add -d <private_key>` удалить конкретный ключ
* `ssh-add -D` удалить все ключи

#### Добавление alias в SSH

```
# ~/.ssh/config
Host webserver
  Hostname 123.123.123.123
  Port 2222
  User mj2278
```

Подключение к вебсерверу `ssh webserver`.

#### Выключить вход по паролю в SSH

Изменить конфигурацию SSH `mcedit /etc/ssh/sshd_config`

```
PasswordAuthentication no
```

Перезагрузить SSH - `systemctl restart ssh`

### UFW (Uncomplicated Firewall)

* `apt install ufw` установка
* `mcedit /etc/default/ufw` настройка
* `ufw default deny incoming` запретить все входящие
* `ufw default allow outgoing` разрешить все исходящие
* `ufw allow ssh` разрешить SSH
* `ufw allow 22` или разрешить SSH (с указанием порта)
* `ufw allow 21/tcp` разрешить порт 21 для TCP
* `ufw allow 6000:6007/udp` разрешить диапазон портов для UDP
* `ufw allow from 15.15.15.51` разрешить подключение только с указанного IP
* `ufw allow from 15.15.15.0/24` разрешить подключение только для IP с 15.15.15.1 по 15.15.15.254
* `ufw enable` включить
* `ufw disable` отключить
* `ufw status verbose` просмотр состояния
* `ufw reset` сбросить все правила
* `ufw delete <number rule>` удалить правила по номеру
* `ufw delete allow http` удалить правило
* `ufw delete allow 80` удалить правило

### DNS

* `dig @1.1.1.1 google.com` узнать ip сайта через DNS 1.1.1.1
* `dig google.com` узнать ip сайта через локальный DNS
* `kdig +tls @1.1.1.1:853 google.com` проверка сайта через DoT (DNS over TLS)
* `dig @cloudflare-dns.com +https example.com` проверка сайта через DoH (DNS over HTTPS)

### Узнать какие порты открыты

```sh
sudo ss -tulnp
```

Расшифровка флагов:

* `-t`: TCP
* `-u`: UDP
* `-l`: слушающие (listening)
* `-n`: не резолвить имена (IP и порты в числах)
* `-p`: показать PID и процесс

### Перевод программы в фоновый режим и возврат к ней (Screen)

* `screen` создает новый скрин в консоли (свернуть скрин Ctr + A, далее нажать D)
* `screen <программа>` запускает программу в новом скрине
* `screen -S <имя скрина> <программа>` запускает программу в скрине с указанным именем
* `screen -r` вернутся к свернутому скрину
* `screen -r <имя скрина>` вернутся к свернутому скрину с указанным именем
* `exit` выход из скрина
* `screen -list` список всех скринов

### Dropbear (подключение по SSH до монтирования диска)

1. Установка

```bash
sudo apt update
sudo apt install dropbear-initramfs busybox
```

2. Настройка

```txt
# /etc/initramfs-tools/initramfs.conf
DEVICE=<указать интерфейс (см ip a)>
IP=dhcp
```

3. Вставить публичный SSH ключ в конфиг

```bash
sudo mcedit /etc/dropbear/initramfs/authorized_keys
sudo chmod 600 /etc/dropbear/initramfs/authorized_keys
```

4. Обновление initramfs

```bash
sudo update-initramfs -u
sudo reboot
```

5. Подключение

```bash
# Подключение под ROOT (настройки сервера SSH менять не нужно, dropbear использует свой SSH-сервер)
ssh root@IP_ТВОЕГО_ПК

# После успешной авторизации
cryptroot-unlock
```

6. Вынести подключение SSH на другой порт, иначе у вас будет две записи авторизации на один хост.

### VNC

1. Установка

```bash
sudo apt update
sudo apt install tigervnc-standalone-server tigervnc-common
```

2. Создание пароля

```bash
vncpasswd
```

3. Настройка VNC-сессии (GNOME или XFCE) 

```bash
mcedit ~/.vnc/xstartup
```

Для XFCE:
```txt
#!/bin/sh
xrdb $HOME/.Xresources
startxfce4 &
```

Для GNOME:
```txt
#!/bin/sh
xrdb $HOME/.Xresources
gnome-session &
```

```bash
chmod +x ~/.vnc/xstartup
```

4. Команды

```bash
# Запуск сервера (номер дисплея VNC порт = 5900 + номер дисплея = порт 5901)
vncserver :1 -localhost no

# Список сессий
vncserver -list

# Остановить сессию
vncserver -kill :1
```

5. Добавить в UFW `ufw allow 5901/tcp`

6. Подключение в MacOS - `vnc://<IP>:5901`

### Отключить Wi-Fi

```bash
sudo nmcli radio wifi off

# Добавим запрет включения
sudo mcedit /etc/NetworkManager/NetworkManager.conf

# Добавь `wifi=off` в секцию `[main]`

# Перезапусти
sudo systemctl restart NetworkManager
```

## Файлы и каталоги

* `find . -type f -exec chmod 644 \{\} \;` рекурсивная установка прав для файлов
* `find . -type d -exec chmod 755 \{\} \;` рекурсивная установка прав для каталогов
* `head <файл>` показать первые 10 строк файла
* `tail -n <кол-во строк> <файл>` показать последние 10 строк файла
* `tac <файл>` читает файл в обратном порядке, удобно использовать для чтения последних записей в файле
* `less <файл>` построчное чтение файла с навигацией
* `cat <файл>` вывод содержимого файла
* `touch <файл>` создание пустого файла
* `file -i <файл>` узнать кодировку файла
* `stat <файл>` информация о файле (размер, права доступа)
* `recode CP1251 <файл>` сменить кодировку файла
* `diff -rq <каталог1> <каталог2>` узнать различия между каталогами
* `ln -s <путь_к_оригиналу> <путь_к_ссылке>` создать символическую ссылку
* `fdupes -rdN <папка>` удаление дубликатов файлов (`sudo apt install fdupes`)

### Форматирование флешки

    df -h
    sudo umount /dev/sdc1
    sudo mkfs.vfat -n 'My flash drive' -I /dev/sdc1

### Работа с диском

* `lsblk` - список дисков и разделов (первый уровень - диски, ветви - разделы дисков);
* `sudo fdisk -l` - список дисков;
* `df -h` информация о смонтированных файловых системах;
* `sudo gdisk <device>` разметка диска (создание разделов), далее `n + Enter` добавление нового раздела;
* `sudo smartctl -H <device>` утилита для мониторинга состояния SSD диска
    * `sudo apt install smartmontools` установка
    * `sudo smartctl -a <device>` полный отчет
* `sudo nvme smart-log <device>` утилита для NVMe-накопителей
    * `sudo apt install nvme-cli` установка

### Проверка целостности файла

    md5sum <файл> > md5sum.txt
    md5sum -c md5sum.txt

## Шифрование

* `truecrypt` зашифрованный контейнер, шифрование раздела диска
* `sudo apt-get install bcrypt`
* `sudo apt-get install ccrypt`

### GPG (GNU Privacy Guard)

* `apt-get install gpg` установка GPG
* `gpg --gen-key` генерация закрытого и открытого ключей
* `gpg --list-keys` вывести список публичных ключей
* `gpg --list-secret-keys` вывести список приватных ключей
* `gpg --export -a -r "Пользователь" > public.key` экспорт публичного ключа
* `gpg --export-secret-key -a -r "Пользователь" > private.key` экспорт приватного ключа
* `gpg --import <файл>` импорт публичного/приватного ключа
* `gpg -e -a -r "Получатель"` зашифровать файл (создаст файл с расширением \*.asc)
* `gpg -e -a -s -r "Получатель"` зашифровать и подписать файл
* `gpg -d -a <файл>` расшифровать файл (бинарные файлы имеют расширение - \*.gpg, тестовые файлы - \*.asc)

В Ubuntu для управления ключами используется программа Seahorse.

Для встраивания возможности шифрования в проводник необходимо установить `apt-get install seahorse-nautilus`.

## Настройка подписи коммитов в GIT с помощью GPG

https://gist.github.com/troyfontaine/18c9146295168ee9ca2b30c00bd1b41e

### Zip (MacOS)

* `zip -er <ZIP file name> <original file name>`
* `unzip <ZIP file name>`

### Посмотреть температуру дисков и процессора

1. Установить lm-sensors

```bash
sudo apt update
sudo apt install lm-sensors
```

2. Найти датчики
Нажимай **Enter** или **Y** на все вопросы.

```bash
sudo sensors-detect
```

3. Посмотреть температуру

```bash
sensors
```

4. Просмотр в реальном времени

```bash
# Обновляет температуру каждые 2 секунды
watch -n 2 sensors
```

## Загрузки

### yt-dlp

* `yt-dlp -f "bestvideo+bestaudio" "URL"` для наилучшего видео и наилучшего аудио
* `yt-dlp -f "bestvideo[ext=mp4]+bestaudio[ext=m4a]/best[ext=mp4]/best" "URL"` явно указать формат
* `yt-dlp -o "/путь/к/папке/%(title)s.%(ext)s" "URL"` скачать в определенную папку
* `yt-dlp -F "URL"` показать доступные форматы перед скачиванием
* `yt-dlp -f "bestaudio" --extract-audio --audio-format mp3 "URL"` только аудио в наилучшем качестве
* `yt-dlp -f "best" "URL"` команда автоматически выберет наилучшее доступное качество
