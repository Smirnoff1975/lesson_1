# lesson_1
ДЗ, занятие №1 (обновление ядра)
# Домашнее задание
Обновление ядра системы
# Цель
научиться обновлять ядро в ОС Linux;
# среда выполнения
VM VirtualBox 7.0.10, OS Ubuntu 24.04.03
# протокол выполнения
-- 1. Смотрим текущую версию ядра - 6.8.0
usr1@srv1:~$ uname -r
6.8.0-134-generic
usr1@srv1:~$

-- 2. Создаем в home папку kernel и переходим в нее
usr1@srv1:~$ mkdir kernel && cd kernel
usr1@srv1:~$

-- 3. Смотрим архитектуру системы, нам надо amd64
usr1@srv1:~/kernel$ uname -p
x86_64
usr1@srv1:~$


-- 4. Пробуем установку 7.1.3 с сайта https://kernel.ubuntu.com/mainline/v7.1.3/, по моему последняя
-- скачиваем все четыре пакета

usr1@srv1:~/kernel$ wget https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-h  eaders-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb
--2026-07-05 17:43:26--  https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-h  eaders-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.74, 185.125.189.7  6, 185.125.189.75
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.74|:443... conne  cted.
HTTP request sent, awaiting response... 200 OK
Length: 4095082 (3.9M) [application/x-debian-package]
Saving to: ‘linux-headers-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.d  eb’

linux-headers-7.1.3 100%[===================>]   3.91M  6.02MB/s    in 0.6s

2026-07-05 17:43:27 (6.02 MB/s) - ‘linux-headers-7.1.3-070103-generic_7.1.3-0701  03.202607041245_amd64.deb’ saved [4095082/4095082]

usr1@srv1:~$


usr1@srv1:~/kernel$ wget https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-headers-7.1.3-070103_7.1.3-070103.202607041245_all.deb
--2026-07-05 17:45:04--  https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-headers-7.1.3-070103_7.1.3-070103.202607041245_all.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.75, 185.125.189.74, 185.125.189.76
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.75|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 14860428 (14M) [application/x-debian-package]
Saving to: ‘linux-headers-7.1.3-070103_7.1.3-070103.202607041245_all.deb’

linux-headers-7.1.3- 100%[====================>]  14.17M  6.71MB/s    in 2.1s

2026-07-05 17:45:07 (6.71 MB/s) - ‘linux-headers-7.1.3-070103_7.1.3-070103.202607041245_all.deb’ saved [14860428/14860428]

usr1@srv1:~/kernel$

usr1@srv1:~/kernel$ wget https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-image-unsigned-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb
--2026-07-05 17:45:36--  https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-image-unsigned-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.76, 185.125.189.74, 185.125.189.75
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.76|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 17490112 (17M) [application/x-debian-package]
Saving to: ‘linux-image-unsigned-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb’

linux-image-unsigned 100%[====================>]  16.68M  8.54MB/s    in 2.0s

2026-07-05 17:45:38 (8.54 MB/s) - ‘linux-image-unsigned-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb’ saved [17490112/17490112]

usr1@srv1:~/kernel$


usr1@srv1:~/kernel$ wget https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-modules-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb
--2026-07-05 17:46:03--  https://kernel.ubuntu.com/mainline/v7.1.3/amd64/linux-modules-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb
Resolving kernel.ubuntu.com (kernel.ubuntu.com)... 185.125.189.75, 185.125.189.76, 185.125.189.74
Connecting to kernel.ubuntu.com (kernel.ubuntu.com)|185.125.189.75|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 170158272 (162M) [application/x-debian-package]
Saving to: ‘linux-modules-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb’

linux-modules-7.1.3- 100%[====================>] 162.28M  10.6MB/s    in 16s

2026-07-05 17:46:20 (10.3 MB/s) - ‘linux-modules-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb’ saved [170158272/170158272]

usr1@srv1:~/kernel$

-- 5. Устанавливаем все пакеты в папке

usr1@srv1:~/kernel$ sudo dpkg -i *.deb
[sudo] password for usr1:
Selecting previously unselected package linux-headers-7.1.3-070103.
(Reading database ... 88403 files and directories currently installed.)
Preparing to unpack linux-headers-7.1.3-070103_7.1.3-070103.202607041245_all.deb ...
Unpacking linux-headers-7.1.3-070103 (7.1.3-070103.202607041245) ...
Selecting previously unselected package linux-headers-7.1.3-070103-generic.
Preparing to unpack linux-headers-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb ...
Unpacking linux-headers-7.1.3-070103-generic (7.1.3-070103.202607041245) ...
Selecting previously unselected package linux-image-unsigned-7.1.3-070103-generic.
Preparing to unpack linux-image-unsigned-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb ...
Unpacking linux-image-unsigned-7.1.3-070103-generic (7.1.3-070103.202607041245) ...
Selecting previously unselected package linux-modules-7.1.3-070103-generic.
Preparing to unpack linux-modules-7.1.3-070103-generic_7.1.3-070103.202607041245_amd64.deb ...
Unpacking linux-modules-7.1.3-070103-generic (7.1.3-070103.202607041245) ...
Setting up linux-headers-7.1.3-070103 (7.1.3-070103.202607041245) ...
Setting up linux-headers-7.1.3-070103-generic (7.1.3-070103.202607041245) ...
Setting up linux-modules-7.1.3-070103-generic (7.1.3-070103.202607041245) ...
Setting up linux-image-unsigned-7.1.3-070103-generic (7.1.3-070103.202607041245) ...
I: /boot/vmlinuz is now a symlink to vmlinuz-7.1.3-070103-generic
I: /boot/initrd.img is now a symlink to initrd.img-7.1.3-070103-generic
Processing triggers for linux-image-unsigned-7.1.3-070103-generic (7.1.3-070103.202607041245) ...
/etc/kernel/postinst.d/initramfs-tools:
update-initramfs: Generating /boot/initrd.img-7.1.3-070103-generic
/etc/kernel/postinst.d/zz-update-grub:
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-7.1.3-070103-generic
Found initrd image: /boot/initrd.img-7.1.3-070103-generic
Found linux image: /boot/vmlinuz-6.8.0-134-generic
Found initrd image: /boot/initrd.img-6.8.0-134-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done
usr1@srv1:~/kernel$

-- 6. Проверяем наличие загрузочной записи нового ядра

usr1@srv1:~/kernel$ ls -al /boot
total 207476
drwxr-xr-x  4 root root     4096 Jul  5 17:51 .
drwxr-xr-x 23 root root     4096 Jul  4 12:53 ..
-rw-r--r--  1 root root   287560 Jun 26 15:31 config-6.8.0-134-generic
-rw-r--r--  1 root root   307299 Jul  4 12:45 config-7.1.3-070103-generic
drwxr-xr-x  5 root root     4096 Jul  5 17:51 grub
lrwxrwxrwx  1 root root       31 Jul  5 17:51 initrd.img -> initrd.img-7.1.3-070103-generic
-rw-r--r--  1 root root 76539996 Jul  4 12:59 initrd.img-6.8.0-134-generic
-rw-r--r--  1 root root 81747083 Jul  5 17:51 initrd.img-7.1.3-070103-generic
lrwxrwxrwx  1 root root       28 Jul  4 12:53 initrd.img.old -> initrd.img-6.8.0-134-generic
drwx------  2 root root    16384 Jul  4 12:48 lost+found
-rw-------  1 root root  9128449 Jun 26 15:31 System.map-6.8.0-134-generic
-rw-------  1 root root 11899898 Jul  4 12:45 System.map-7.1.3-070103-generic
lrwxrwxrwx  1 root root       28 Jul  5 17:51 vmlinuz -> vmlinuz-7.1.3-070103-generic
-rw-------  1 root root 15042952 Jun 26 16:36 vmlinuz-6.8.0-134-generic
-rw-------  1 root root 17453568 Jul  4 12:45 vmlinuz-7.1.3-070103-generic
lrwxrwxrwx  1 root root       25 Jul  4 12:53 vmlinuz.old -> vmlinuz-6.8.0-134-generic
usr1@srv1:~/kernel$

-- 7. Обновляем загрузчик grub, чтобы появилачь новая запись в загрузочном меню и делаем ее по умолчанию, затем перезагружаем систему.

usr1@srv1:~/kernel$ sudo update-grub
Sourcing file `/etc/default/grub'
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-7.1.3-070103-generic
Found initrd image: /boot/initrd.img-7.1.3-070103-generic
Found linux image: /boot/vmlinuz-6.8.0-134-generic
Found initrd image: /boot/initrd.img-6.8.0-134-generic
Warning: os-prober will not be executed to detect other bootable partitions.
Systems on them will not be added to the GRUB boot configuration.
Check GRUB_DISABLE_OS_PROBER documentation entry.
Adding boot menu entry for UEFI Firmware Settings ...
done
usr1@srv1:~/kernel$
usr1@srv1:~/kernel$ sudo grub-set-default 0
usr1@srv1:~/kernel$ sudo reboot

-- 8. Проверяем результат.

usr1@srv1:~$ uname -r
7.1.3-070103-generic
usr1@srv1:~$

