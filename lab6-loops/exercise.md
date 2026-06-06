- В данный момент playbook запускает команду, которая выводит на экран название фрукта. Примени цикл (директиву `with_items`) в этом task, чтобы отобразились все фрукты из списка преременной `fruits`.

##### Решение

```
-
    name: 'Print list of fruits'
    hosts: localhost
    vars:
        fruits:
            - Apple
            - Banana
            - Grapes
            - Orange
    tasks:
        -
            command: 'echo "{{ item }}"'
            with_items: '{{ fruits }}'
```

- Более реалистичный вариант использования. Нам нужно установить несколько пакетов с помощью модуля `yum`. Текущий playbook устанавливает только отдельный пакет.
##### Решение

```
-
    name: 'Install required packages'
    hosts: localhost
    vars:
        packages:
            - httpd
            - binutils
            - glibc
            - ksh
            - libaio
            - libXext
            - gcc
            - make
            - sysstat
            - unixODBC
            - mongodb
            - nodejs
            - grunt
    tasks:
        -
            yum:
                name: '{{ item }}'
                state: present
            with_items: '{{ packages }}'
```

