Задание на перезапуск нескольких серверов в определенной последовательности. Последовательность и команды для исполнения приведены ниже. Обрати внимание, что эти команды нужно запустить только на подходящих для этого серверах. Изучи inventory-файл и обнови playbook, чтобы последовательность была правильной.

> ИНФО: Используй приведенное ниже описание для `plays` и `tasks`.

1. `Останови` `web`-службы на веб-серверах - `service httpd stop`
2. `Останови` `db`-службы на серверах баз данных - `service mysql stop`
3. `Перегрузи` `все` сервера (`web` и `db`) за один раз - `/sbin/shutdown -r`
4. `Запусти` `db`-службы на узлах баз данных - `service mysql start`
5. `Запусти` `web`-службы на узлах веб-серверов - `service httpd start`

### Решение

```
-
    name: 'Stop the web services'
    hosts: web_nodes
    tasks:
        -
            name: 'Stop the web services on web server nodes'
            command: 'service httpd stop'
-
    name: 'Shutdown the database'
    hosts: db_nodes
    tasks:
        -
            name: 'Shutdown the database services on db server nodes'
            command: 'service mysql stop'
-
    name: 'Restart all servers '
    hosts: all_nodes
    tasks:
        -
            name: 'Restart all servers (web and db) at once'
            command: '/sbin/shutdown -r'
-
    name: 'Start the database services on db server nodes'
    hosts: db_nodes
    tasks:
        -
            name: 'Start the database services on db server nodes'
            command: 'service mysql start'
-
    name: 'Start the web services on web server nodes'
    hosts: web_nodes
    tasks:
        -
            name: 'Start the web services on web server nodes'
            command: 'service httpd start'

```