Linux
=====

Просмотр информации
-------------------

* `free -m` количество оперативной памяти
* `df -h` количество места на жестком диске
* `lscpu` архитектура процессора
* `uptime` нагрузка системы за 1, 5, 15 минут
* `ps -u <пользователь> fu` процессы пользователя в виде дерева
* `which <программа>` узнать каталог в котором физически находится исполняемый файл программы
* `hexdump -C <файл>` побайтовый просмотр файла

Выключение, запуск программ
---------------------------

* `shutdown -h now` выключение системы
* `restart` перезагрузка

Установка программ
------------------

* `sudo apt-get install logrotate` ротация логов
* `sudo apt-get install goaccess` анализ логов сервера
* `sudo apt-get install ntp` утилита синхронизации времени по протоколу NTP
 
### Установка NodeJS

```bash
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs
```

### Установка Sublime Text

```bash
sudo add-apt-repository ppa:webupd8team/sublime-text-3
sudo apt-get update
sudo apt-get install sublime-text-installer
```

### Установка шрифтов Microsoft

```bash
sudo rm -rf /var/lib/update-notifier/package-data-downloads/partial/*
sudo apt-get --purge --reinstall install ttf-mscorefonts-installer
sudo fc-cache -fv
```

### Установка OpenVPN Client

```bash
sudo apt install network-manager-openvpn network-manager-openvpn-gnome
sudo systemctl restart NetworkManager.service
```

### Установка Debian 10 (из Mac OS)

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
sudo dd if=debian-10.5.0-amd64-netinst.img of=/dev/disk2 bs=1m
```

Настройка
---------

* `sudo dpkg-reconfigure tzdata` смена временной зоны
* `sudo update-alternatives --config editor` задать текстовый редактор по умолчанию
* `sudo apt-get install gnome-tweak-tool` вернуть минимизацию окна, "Заголовки окон" -> "Кнопки заголовка окна"

Графическая оболочка
--------------------

* `systemctl get-default` текущее состояние
* `systemctl set-default multi-user.target` загрузка Linux без графической оболочки
* `systemctl set-default graphical.target` загрузка Linux с графической оболочкой

Пользователи
------------

* `adduser <имя>` добавить пользователя
* `visudo` разрешить запуск команд суперпользователя из под sudo

Сеть
----


Работа с SSH
------------

* `ssh-keygen` создание публичного и приватного ключей
* `ssh-copy-id -i ~/.ssh/id_rsa.pub username@host` передача открытого ключа на другой компьютер
* `ssh -L 8000:192.168.0.100:80 user@host` переброс удаленного порта на локальный (например получить доступ к локальному сайту)
* `ssh -i <private_key.txt> username@host` подключение через приватный ключ

### Перевод программы в фоновый режим и возврат к ней (Screen)

* `screen` создает новый скрин в консоли (свернуть скрин Ctr + A, далее нажать D)
* `screen <программа>` запускает программу в новом скрине
* `screen -S <имя скрина> <программа>` запускает программу в скрине с указанным именем
* `screen -r` вернутся к свернутому скрину
* `screen -r <имя скрина>` вернутся к свернутому скрину с указанным именем
* `exit` выход из скрина
* `screen -list` список всех скринов


Файлы и каталоги
----------------

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

### Форматирование флешки

    df -h
    sudo umount /dev/sdc1
    sudo mkfs.vfat -n 'My flash drive' -I /dev/sdc1

### Проверка целостности файла

    md5sum <файл> > md5sum.txt
    md5sum -c md5sum.txt

Шифрование
----------

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
