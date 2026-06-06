- В playbook нужно добавить play для выполнения скрипта на всех веб-серверах. Название у play будет `Execute a script on all web server nodes`. Сам скрипт находится в размещении `/tmp/install_script.sh`
- Измени playbook, добавив в него новый task, который запустит сервис `httpd` на всех `web`-хостах
- Обнови playbook и добавь в него новый task, так, чтобы он был первым. Этот task должен вносить запись в файл `/etc/resolv.conf` для этих `web_nodes`. Строка, которую надо добавить: `nameserver 10.1.250.10`

> ИНФО: Новый task должен быть выполнен `первым`, так что запиши его в соответствующем месте.

- Измени playbook и добавь в него новый task на вторую позицию (как раз после добавления строки в `resolv.conf`), который будет создавать нового пользователя на веб-сервере.

### Решение

```
-
    name: 'Execute a script on all web server nodes and start httpd service'
    hosts: web_nodes
    tasks:
        -
            name: 'Update entry into /etc/resolv.conf'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
        -
            name: 'Create a new user'
            user:
                name: web_user
                uid: 1040
                group: developers
        -
            name: 'Execute a script'
            script: /tmp/install_script.sh
        -
            name: 'Start httpd service'
            service:
                name: httpd
                state: present

```