Ubuntu
======

Сочетание клавиш
----------------

| Символ | Сочетание клавиш                        |
|--------|-----------------------------------------|
| ®      | compose_key o r                         |
| ©      | compose_key o c                         |
| ™      | compose_key t m                         |
| ¼      | compose_key 1 4                         |
| ½      | compose_key 1 2                         |
| €      | compose_key e =                         |
| ¥      | compose_key y =                         |
| ≠      | compose_key / =                         |
| °      | compose_key o o                         |
| «      | compose_key right_shift+< right_shift+< |
| »      | compose_key right_shift+> right_shift+> |
| ¿      | compose_key right_shift+? right_shift+? |
| ←      | compose_key left_shift+< -              |
| …      | compose_key . .                         |

Установка драйвера видеокарты
-----------------------------

```bash
sudo apt-get install nvidia-331 nvidia-settings nvidia-prime
```

Если не работает регулировка яркости экрана на ASUS
---------------------------------------------------

- Modify GRUP parameters (/etc/default/grub)
- Added "acpi_osi=" in GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_osi="

Если не работает передача файлов через bluetooth
------------------------------------------------

```bash
sudo gedit /etc/bluetooth/main.conf
```

Change `EnableGatt = true`
