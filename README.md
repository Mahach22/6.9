
**Установите Ansible на вашу локальную машину, проверьте, что Ansible установлен корректно.** <br>
---

**Практическое задание-1: Написание плейбука для установки веб-сервера.** <br>
**1. Напишите плейбук Ansible, который устанавливает веб-сервер Apache на удалённом хосте.** <br>
**2. Убедитесь, что плейбук работает корректно, запустив его на тестовом сервере. Убедитесь, что Apache установлен на указанном хосте.** <br>







```
- name: Install Apache web server
  hosts: all
  become: yes
  tasks:
    - name: Install Apache package
      apt:
        name: apache2
        state: present
        update_cache: yes

    - name: Ensure Apache service is running
      service:
        name: apache2
        state: started
        enabled: yes
```

```
ansible-playbook -i inventory_file install_apache.yml
```



---

**Практическое задание-2: Использование модулей для настройки системы.** <br>
**1. Используйте модуль Ansible для создания нового пользователя на удалённом сервере.** <br>
**2. Настройте разрешения для этого пользователя с помощью другого модуля.** <br>

```
- name: Create new user and set permissions
  hosts: all
  become: yes
  tasks:
    - name: Create a new user
      user:
        name: newuser
        state: present
        shell: /bin/bash

    - name: Set permissions for the new user
      file:
        path: /home/newuser
        owner: newuser
        group: newuser
        mode: '0755'

```

```
ansible-playbook -i inventory_file create_user.yml
```






---

**Практическое задание-3: Создание плейбука с условиями и циклам.** <br>
**1. Напишите плейбук, который устанавливает пакет только если он ещё не установлен.** <br>
**2. Используйте цикл для создания нескольких файлов с разными именами.** <br>



```
- name: Install a package if not already installed and create multiple files
  hosts: all
  become: yes
  tasks:
    - name: Install package only if not installed
      apt:
        name: vim
        state: present
      when: ansible_facts.packages['vim'] is not defined

    - name: Create multiple files with different names
      file:
        path: "/tmp/file_{{ item }}.txt"
        state: touch
      loop:
        - 1
        - 2
        - 3
        - 4
        - 5

```

```
ansible-playbook -i inventory_file install_and_create_files.yml
```
