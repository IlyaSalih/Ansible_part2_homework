# Домашнее задание к занятию "`Ansible part 2`" - `Salihzyanov Ilyas`

### Задание 1

`Выполните действия, приложите файлы с плейбуками и вывод выполнения.`

`Напишите три плейбука. При написании рекомендуем использовать текстовый редактор с подсветкой синтаксиса YAML.`

`Плейбуки должны:`

1. `Скачать какой-либо архив, создать папку для распаковки и распаковать скаченный архив. Например, можете использовать официальный сайт и зеркало Apache Kafka. При этом можно скачать как исходный код, так и бинарные файлы, запакованные в архив — в нашем задании не принципиально.`
2. `Установить пакет tuned из стандартного репозитория вашей ОС. Запустить его, как демон — конфигурационный файл systemd появится автоматически при установке. Добавить tuned в автозагрузку.`
3. `Изменить приветствие системы (motd) при входе на любое другое. Пожалуйста, в этом задании используйте переменную для задания приветствия. Переменную можно задавать любым удобным способом.`

1. `mkdir ~/ansible/task_1`
2. `cd ~/ansible/task_1`
3. `touch playbook_1,yml | touch playbook_2.yml | touch playbook_3.yml | touch inventory.ini`
4. `Open in Visual studio code > file > open folder > ~/ansible/task_1`

## playbook_1
```
---
- name: playbook_1
  hosts: all

  tasks:

   - name: download Kafka
     get_url:
      url: https://www.apache.org/dyn/closer.lua/kafka/4.2.0/kafka-4.2.0-src.tgz?action=download
      dest: /tmp/kafka-4.2.0-src.tgz

   - name: create directory
     ansible.builtin.file:
      path: /home/ansible/task_1/extract_kafka
      state: directory
      mode: 0755

   - name: extract Kafka
     unarchive:
      src: /tmp/kafka-4.2.0-src.tgz
      dest: /home/ansible/task_1/extract_kafka
      remote_src: true
...
```

# ansible-playbook -i inventory.ini playbook_1.yml

PLAY [playbook_1] **************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [localhost]

TASK [download Kafka] **********************************************************************************************************************************
changed: [localhost]

TASK [create directory] ********************************************************************************************************************************
ok: [localhost]

TASK [extract Kafka] ***********************************************************************************************************************************
ok: [localhost]

PLAY RECAP *********************************************************************************************************************************************
localhost                  : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

## playbook_2
```
---
- name: playbook_2
  hosts: all
  become: yes

  tasks:

      - name: install tuned
        ansible.builtin.apt:
          name: tuned
          state: present

      - name: run tuned
        ansible.builtin.systemd:
          name: tuned
          state: started
          enabled: yes
...
```

# ansible-playbook -i inventory.ini playbook_2.yml  -K

PLAY [playbook_2] **************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [localhost]

TASK [install tuned] ***********************************************************************************************************************************
ok: [localhost]

TASK [run tuned] ***************************************************************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************************************************
localhost                  : ok=3    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

## playbook_3
```
---
- name: playbook_3
  hosts: all
  become: yes

  vars:
    motd_message: "Hey teacher, do you like my playbooks?)"

  tasks:

    - name: change motd
      copy:
        content: "{{ motd_message }}"
        dest: /etc/motd
...
```

# ansible-playbook -i inventory.ini playbook_3.yml  -K

PLAY [playbook_3] **************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [localhost]

TASK [change motd] *************************************************************************************************************************************
ok: [localhost]

PLAY RECAP *********************************************************************************************************************************************
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0


# cat /etc/motd

Hey teacher, do you like my playbooks?)host@host:~/ansible/task_1$

![screenshot-1](playbook_1.png)
![screenshot-2](playbook_2.png)
![screenshot-3](playbook_3.png)



---

### Задание 2

`Задание 2`
`Выполните действия, приложите файлы с модифицированным плейбуком и вывод выполнения.`

`Модифицируйте плейбук из пункта 3, задания 1. В качестве приветствия он должен установить IP-адрес и hostname управляемого хоста, пожелание хорошего дня системному администратору.`

1. `ansible localhost -m setup | grep ansible_hostname`
2. `ansible localhost -m setup | grep ansible_default_ipv4`
3. `ansible localhost -m setup | grep ansible_default_ipv4 -A 5`
4. `change playbook_3 in visual studio code`

## playbook_3
```
---
- name: playbook_3
  hosts: all
  become: yes

  vars:
    motd_message: |
      Hostname: {{ ansible_hostname }}
      IP address: {{ ansible_default_ipv4.address }}

      Have a nice day, administator)

  tasks:

    - name: change motd
      copy:
        content: "{{ motd_message }}"
        dest: /etc/motd
...
```

# ansible-playbook -i inventory.ini playbook_3.yml  -K

PLAY [playbook_3] **************************************************************************************************************************************

TASK [Gathering Facts] *********************************************************************************************************************************
ok: [localhost]

TASK [change motd] *************************************************************************************************************************************
changed: [localhost]

PLAY RECAP *********************************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

# cat /etc/motd

Hostname: host
IP address: 10.0.2.15

Have a nice day, administator)

![screenshot-1](ansible localhost -m setup.png)
![screenshot-2](changed playbook_3.png)
![screenshot-3](cat.png)

### Задание 3

`Приведите ответ в свободной форме........`

1. `Заполните здесь этапы выполнения, если требуется ....`
2. `Заполните здесь этапы выполнения, если требуется ....`
3. `Заполните здесь этапы выполнения, если требуется ....`
4. `Заполните здесь этапы выполнения, если требуется ....`
5. `Заполните здесь этапы выполнения, если требуется ....`
6.

```
Поле для вставки кода...
....
....
....
....
```

`При необходимости прикрепитe сюда скриншоты
![Название скриншота](ссылка на скриншот)`
