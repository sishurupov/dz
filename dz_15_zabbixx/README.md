Настроить дашборд с 4-мя графиками

память;
процессор;
диск;
сеть.
Настроить на одной из систем:

zabbix (использовать screen (комплексный экран);
prometheus - grafana.

Ход работ:

1. Создание конфигурации Docker Compose: Написал docker-compose файл для запуска нескольких контейнеров:

Zabbix Server (порт 10051)
        
Zabbix PostgreSQL Database (порт 5432)
        
Zabbix Nginx (порт 8080)
        
Grafana (порт 3000)

2. Запуск контейнеров: Запустил контейнеры с помощью Docker Compose.

3. Установка агентов Zabbix: Установил Zabbix-агенты на сервер и хост, где находится сервер.

4. Настройка Zabbix-сервера:

Добавил пассивные Zabbix-прокси в настройках Zabbix. Добавленные Zabbix-прокси

![image](https://github.com/user-attachments/assets/e3ada36c-7054-42bb-8fca-7965e3fca146)

Добавил хосты с установленными агентами.

![image](https://github.com/user-attachments/assets/6d521f5d-f19f-43fd-84f5-0404c19d80d6)


5. Развертывание Docker Swarm: Развернул кластер Docker Swarm на двух других ВМ с пассивными Zabbix-прокси.

6. Добавление шаблонов в Zabbix: Назначил необходимые шаблоны на хосты в Zabbix.

7. Настройка источников данных в Grafana: Добавил источники данных PostgreSQL и Zabbix сервера в Grafana. Настройка источников данных_

![image](https://github.com/user-attachments/assets/f0b39487-d783-45fe-8cb4-285691ae2985)

8. Создание дашборда в Grafana: Создал новый дашборд и добавил 4 панели (утилизация ЦПУ, ОЗУ, диск, сеть)

   ![image](https://github.com/user-attachments/assets/2304db80-c0a6-49d6-9129-35fa8e55f962)


