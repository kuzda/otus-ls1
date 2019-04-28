### Установка необходимых пакетов
*sudo yum groupinstall -y "Development-tools"
sudo yum install -y ncurses-devel hmaccalc zlib-devel binutils-devel elfutils-libelf-devel openssl-devel bc wget*

### Скачиваем и распаковываем ядро
*cd /usr/src
wget https://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.0.9.tar.xz
tar -xfv linux-5.0.9.tar.xz*
Для удобства можно создать симлинк на директорию с исходниками ядра

### Конфигурируем ядро (при необходимости), собираем его, собираем и устанавливаем модули
*make mrproper* (вдруг что-то еще делали)
*make menuconfig* (открываем псевдографический интерфейс)
Я отключил ненужные драйвера беспроводных систем, поддержку некотроых ФС, параметров более чем дофига, простор для фантазии не ограничен.
*make dep* (устанавливаем зависимости)
*make bzImage* (создаем образ ядра)
*make modules* (собираем модули)
*make modules_install* (устанавливаем модули)
*make install* (наконец-то ставим ядро)

### Меняем конфиг grub для загрузки с нового ядра
Для этого смотрим в boot/grub2/grub.cfg и находим наше ядро:
*cat /boot/grub2/grub.cfg | grep menuentry*
Меняем параметр GRUB_DEFAULT=SAVED в /etc/default/grub на порядковый номер записи с нашим ядром в конфиге grub, обычно это 0
*sudo nano /etc/default/grub*
Собираем новый конфиг grub
*grub2-mkconfig -o /boot/grub2/grub.cfg*

Теперь можно перезагружаться с новым ядром.






