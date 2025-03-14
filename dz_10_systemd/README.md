Выполнить следующие задания и подготовить развёртывание результата выполнения с использованием Vagrant и Vagrant shell provisioner (или Ansible, на Ваше усмотрение):

1 Написать service, который будет раз в 30 секунд мониторить лог на предмет наличия ключевого слова (файл лога и ключевое слово должны задаваться в /etc/default).

2 Установить spawn-fcgi и создать unit-файл (spawn-fcgi.sevice) с помощью переделки init-скрипта (https://gist.github.com/cea2k/1318020).

3 Доработать unit-файл Nginx (nginx.service) для запуска нескольких инстансов сервера с разными конфигурационными файлами одновременно.




Ход работ:


1. В Vagrantfile:
Скрипт 1.sh создает мониторинговый скрипт /usr/local/bin/log_monitor.sh, который проверяет файл лога на наличие ключевого слова каждые 30 секунд.
Скрипт 2.sh создает конфигурационный файл /etc/default/log_monitor с параметрами для мониторинга (путь к логу и ключевое слово info).
Скрипт 3.sh создает unit-файл log_monitor.service для запуска мониторингового скрипта как системной службы.
Сервис log_monitor запускается с помощью команд systemctl enable и systemctl start.
Проверили, что сервис работает, командой journalctl -u log_monitor.service и добавили тестовое сообщение в лог-файл, чтобы убедиться, что сервис его обнаруживает.

![image](https://github.com/user-attachments/assets/d3a58bc1-63fb-4c77-81a8-2ca21cafecfb)


2. В Vagrantfile:
Так как spawn-fcgi недоступен в CentOS 9, мы использовали fcgiwrap в качестве альтернативы.
Скрипт 4.sh создает unit-файл fcgiwrap.service, который запускает fcgiwrap как службу, настроив его на использование TCP-порта 9000.
Запускается fcgiwrap с помощью команд systemctl enable и systemctl start.
Проверяется статус службы командой systemctl status fcgiwrap и выполняются тестовые запросы с помощью curl по адресам http://localhost:8080/cgi-bin/test.cgi и http://localhost:8081/cgi-bin/test.cgi.

![image](https://github.com/user-attachments/assets/a798e323-2e5b-4e57-9183-b34dad332b2e)


3. В Vagrantfile:
Скрипт 5.sh создает unit-файл nginx@.service, который позволяет запускать несколько экземпляров Nginx с разными конфигурационными файлами.
Настроены уникальные порты и PID-файлы для каждого экземпляра Nginx.
Скрипт 6.sh добавляет нужные настройки для CGI в конфигурационные файлы Nginx.
Скрипт 7.sh создает тестовый CGI-сценарий /usr/share/nginx/html/cgi-bin/test.cgi.
Запустили и проверили оба экземпляра Nginx с помощью команд systemctl enable, systemctl start и systemctl status.
Убедились, что оба экземпляра Nginx работают корректно, и CGI-сценарии выполняются, используя curl.


