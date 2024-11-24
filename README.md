
**Установите Ansible на вашу локальную машину, проверьте, что Ansible установлен корректно.** <br>
---

**Практическое задание-1: Написание плейбука для установки веб-сервера.** <br>
**1. Напишите плейбук Ansible, который устанавливает веб-сервер Apache на удалённом хосте.** <br>
**2. Убедитесь, что плейбук работает корректно, запустив его на тестовом сервере. Убедитесь, что Apache установлен на указанном хосте.** <br>

Создаем плейбук для установки Apache

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
Применяем плейбук
```
ansible-playbook -i /etc/ansible/hosts install_apache.yml -l all --user root

```
![11](https://github.com/Mahach22/6.9/blob/main/1.1apache.install.png)

Проверяем курлом результат установки Apache

![12](https://github.com/Mahach22/6.9/blob/main/1.2.apache.curl.png)


---

**Практическое задание-2: Использование модулей для настройки системы.** <br>
**1. Используйте модуль Ansible для создания нового пользователя на удалённом сервере.** <br>
**2. Настройте разрешения для этого пользователя с помощью другого модуля.** <br>

Создаем плейбук для создания нового пользователя и выдачи прав на его домашнюю директорию
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
Применяем созданный плейбук
```
ansible-playbook -i /etc/ansible/hosts create_user.yml -l all --user root
```

![21](https://github.com/Mahach22/6.9/blob/main/2.1.user.and.permissions.png)

Проверяем результат непосредственно с машины, на которой производились работы

![22](https://github.com/Mahach22/6.9/blob/main/2.2.permissions.remote.machine.png)

---

**Практическое задание-3: Создание плейбука с условиями и циклами.** <br>
**1. Напишите плейбук, который устанавливает пакет только если он ещё не установлен.** <br>
**2. Используйте цикл для создания нескольких файлов с разными именами.** <br>

Создаем плейбук, который установит vim, если он не был установлен, а также создаст 5 текстовых файлов с использованием цикла.

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
        path: "/home/newuser/file_{{ item }}.txt"
        state: touch
      loop:
        - 1
        - 2
        - 3
        - 4
        - 5

```

Применяем наш плейбук

```
ansible-playbook -i /etc/ansible/hosts install_and_create_files.yml -l all --user root
```

![31](https://github.com/Mahach22/6.9/blob/main/3.install.and.create.png)

Проверяем результат создания файлов с машины, на которой производились работы

![32](https://github.com/Mahach22/6.9/blob/main/3.created.files.png)
