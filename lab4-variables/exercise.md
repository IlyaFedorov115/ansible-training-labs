- Этот playbook используется для обновления записей о неймсерверах в файле `resolv.conf` на localhost. Информация о нужном неймсервере также представлена в inventory-файле в переменнной `nameserver_ip`. Обратись к inventory за подробностями.

- Замени IP неймсервера в этом playbook, чтобы использовалось значение переменной `nameserver_ip` из inventory-файла, таким образом в будущем, когда потребуется внести изменения ты легко сможешь поправить лишь файл inventory.

- Мы добавили в playbook новый task для отключения SNMP порта. Однако значение порта захардкожено в этом task. Измени файл inventory и добавь новую переменную `snmp_port` и присвой ей значение, как указано в этом playbook. После обнови playbook, чтобы он использовал значение этой переменной для своей работы.

### Решение

```
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Update nameserver entry into resolv.conf file'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver {{ nameserver_ip }}'
        -
            name: 'Disable SNMP Port'
            firewalld:
                port: '{{ snmp_port }}'
                permanent: true
                state: disabled
```

- Мы напечатали некоторою персональную информацию на экране. Мы бы хотели перенести `car_model`, `country_name` and `title` в переменные, определенные на уровне play.

Создай три новые пременные (`car_model`, `country_name` and `title`) на уровне play и присвой им значения, какими они были в исходном playbook. Далее используй эти переменные в tasks.


### Решение

```
-
    hosts: localhost
    vars:
        car_model: 'BMW M3'
        country_name: USA
        title: 'Systems Engineer'
    tasks:
        -
            command: 'echo "My car''s model is {{ car_model }}"'
        -
            command: 'echo "I live in the {{ country_name }}"'
        -
            command: 'echo "I work as a {{ title }}"'

```