Что нужно сделать?

Определить алгоритм с наилучшим сжатием:
Определить какие алгоритмы сжатия поддерживает zfs (gzip, zle, lzjb, lz4);
создать 4 файловых системы на каждой применить свой алгоритм сжатия;
для сжатия использовать либо текстовый файл, либо группу файлов.
Определить настройки пула.
С помощью команды zfs import собрать pool ZFS.
Командами zfs определить настройки:
   
- размер хранилища;
    
- тип pool;
    
- значение recordsize;
   
- какое сжатие используется;
   
- какая контрольная сумма используется.
Работа со снапшотами:
скопировать файл из удаленной директории;
восстановить файл локально. zfs receive;
найти зашифрованное сообщение в файле secret_message.

Ход работ:

1. Определить алгоритм с наилучшим сжатием:

Создал 4 пула из 4 дисков специально под каждый тип сжатия в файловой системе, применил типы сжатия с помощью zfs set compression.

Закинул в каждую ФС заранее созданные 2 файла различных типов (txt и csv), а также изображение dng.

После чего при помощи цикла получил данные о размере каждого файла (результаты на скриншотах).

ZLE практически не сжимал наши данные, вне зависимости от типа файла.

Изображение по сути смог сжать только gzip.

![image](https://github.com/user-attachments/assets/13eac0b7-60fc-44d5-a020-e4e0c1955dfb)

TXT файлы лучше всего были сжаты gzip (26кб) и lz4 (47кб).

![image](https://github.com/user-attachments/assets/16246fd9-a0cc-48b9-b38a-f1d0d628d3f9)


С CSV файлами также лучше всех справился gzip, с почти двухкратным преимуществом от остальных типов.

![image](https://github.com/user-attachments/assets/da4a19a1-8273-442f-9bd9-bdd9e5ed56ba)


2. Определить настройки пула.

Размер хранилища:

zfs get available otus

NAME  PROPERTY   VALUE  SOURCE

otus  available  350M   -

Тип pool

 zpool status otus
  pool: otus
  state: ONLINE
  status: Some supported and requested features are not enabled on the pool.
          The pool can still be used, but some features are unavailable.
  action: Enable all features using 'zpool upgrade'. Once this is done,
          the pool may no longer be accessible by software that does not support
          the features. See zpool-features(7) for details.
  config:

          NAME                         STATE     READ WRITE CKSUM
          otus                         ONLINE       0     0     0
            mirror-0                   ONLINE       0     0     0
              /home/zpoolexport/filea  ONLINE       0     0     0
              /home/zpoolexport/fileb  ONLINE       0     0     0

Значение recordsize

 zfs get recordsize otus
 
NAME  PROPERTY    VALUE    SOURCE

otus  recordsize  128K     local

Какое сжатие используется

zfs get compression otus

NAME  PROPERTY     VALUE           SOURCE

otus  compression  zle             local

Какая контрольная сумма используется

 zfs get checksum otus
 
NAME  PROPERTY  VALUE      SOURCE

otus  checksum  sha256     local

![image](https://github.com/user-attachments/assets/72b3e8e2-6bf3-4fa2-a64c-60a91c5c0e03)


3. Работа со снапшотами:
скопировать файл из удаленной директории;
восстановить файл локально. zfs receive;
найти зашифрованное сообщение в файле secret_message.

При помощи zfs receive и cat получил зашифрованное сообщение.

![image](https://github.com/user-attachments/assets/60757c7e-655d-4709-a300-e1ecb31fc591)

