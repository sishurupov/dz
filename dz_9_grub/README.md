
Включить отображение меню Grub.
Попасть в систему без пароля несколькими способами.
Установить систему с LVM, после чего переименовать VG.


1. Включить отображение меню Grub.

Открываем через vi файл /etc/default/grub и редактируем следующие параметры:

GRUB_TIMEOUT=7 

GRUB_TIMEOUT_STYLE=menu 

GRUB_CMDLINE_LINUX="biosdevname=0 no_timer_check vga=792 nomodeset text crashkernel=1G-4G:192M,4G-64G:256M,64G-:512M resume=/dev/mapper/centotus-swap rd.lvm.lv=centotus/root rd.lvm.lv=centotus/swap net.ifnames=0"

Jбновляем конфигурацию Grub:

sudo grub2-mkconfig -o /boot/grub2/grub.cfg

2. Попасть в систему без пароля несколькими способами

   init=/bin/bash в Grub
При перезагрузке системы, на экране Grub нажимаем e у нужного ядра, и в появившемся меню находим строку с началом на linux и в самом конце добавляем init=/bin/bash. После этого нажимаем Ctrl+X и заходим в систему без пароля.

![image](https://github.com/user-attachments/assets/a301b409-a545-4ed5-81f4-5a0d256713b4)


Live CD для сброса

В нашей виртуальной машине в VirtualBox добавляем новый оптический диск с образом CentOS 9 Stream, меняем приоритет загрузки на него, заходим в Troubleshooting → Rescue a CentOS Linux system. Выбираем 1 для продолжения, затем выполняем:

chroot /mnt/системный корень

Меняем пароль root через passwd, устанавливаем touch /.autorelabel, так как у нас включен SELinux. После этого перезагружаем систему и заходим под новым паролем.

![image](https://github.com/user-attachments/assets/9c294e17-3a25-4bef-9724-c455bdbf349c)

3. Установить систему с LVM, после чего переименовать VG.

Поскольку мы используем стандартный CentOS 9 Stream, мы сразу получаем LVM для корневого каталога /. Осталось только узнать и изменить название VG

С помощью vgrename меняем название на centotus. Затем обновляем /etc/fstab, чтобы не было записей о старом названии centos9s.

![image](https://github.com/user-attachments/assets/3537260b-f66e-4342-9867-1b4227c588dc)

После этого выполняем:

sudo dracut -f -v
