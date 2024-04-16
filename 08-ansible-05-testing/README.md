# Домашнее задание к занятию 5 «Тестирование roles»

## Подготовка к выполнению

1. Установите molecule и его драйвера: `pip3 install "molecule molecule_docker molecule_podman`.
2. Выполните `docker pull aragast/netology:latest` —  это образ с podman, tox и несколькими пайтонами (3.7 и 3.9) внутри.

## Основная часть

Ваша цель — настроить тестирование ваших ролей. 

Задача — сделать сценарии тестирования для vector. 

Ожидаемый результат — все сценарии успешно проходят тестирование ролей.

### Molecule

1. Запустите  `molecule test -s ubuntu_xenial` (или с любым другим сценарием, не имеет значения) внутри корневой директории clickhouse-role, посмотрите на вывод команды. Данная команда может отработать с ошибками или не отработать вовсе, это нормально. Наша цель - посмотреть как другие в реальном мире используют молекулу И из чего может состоять сценарий тестирования.
2. Перейдите в каталог с ролью vector-role и создайте сценарий тестирования по умолчанию при помощи `molecule init scenario --driver-name docker`.

![Scenario Init](img/HW5_scenario_init.png)

3. Добавьте несколько разных дистрибутивов (oraclelinux:8, ubuntu:latest) для инстансов и протестируйте роль, исправьте найденные ошибки, если они есть.

<details>
  <summary>molecule.yml</summary>

```bash
---
dependency:
  name: galaxy
driver:
  name: docker
lint: |
  ansible-lint .
  yamllint .
platforms:
  - name: centos7
    image: docker.io/pycontribs/centos:7
    pre_build_image: true
  - name: ubuntu
    image: docker.io/pycontribs/ubuntu:latest
    pre_build_image: true
  - name: oraclelinux
    image: docker.io/oraclelinux:8
    pre_build_image: true
provisioner:
  name: ansible
verifier:
  name: ansible
  enabled: True
role_name_check: 1
```
</details>

Запуск тестирования
<details>
  <summary>molecule test</summary>

```bash
[root@ansible01 vector-role]# molecule test --destroy=never
WARNING  Driver docker does not provide a schema.
INFO     default scenario test matrix: dependency, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=1...
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
WARNING  Skipping, '--destroy=never' requested.
INFO     Running default > syntax
INFO     Sanity checks: 'docker'

playbook: /root/mnt-homeworks/08-ansible-04-role/playbook/roles/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 2, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/ubuntu:latest)
skipping: [localhost] => (item=molecule_local/docker.io/oraclelinux:8)
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=oraclelinux)

TASK [Wait for instance(s) creation to complete] *******************************
ok: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j771755846344.4507', 'results_file': '/root/.ansible_async/j771755846344.4507', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
ok: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j154363649494.4532', 'results_file': '/root/.ansible_async/j154363649494.4532', 'changed': True, 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
ok: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j556888759507.4564', 'results_file': '/root/.ansible_async/j556888759507.4564', 'changed': True, 'item': {'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [oraclelinux]
ok: [ubuntu]
ok: [centos7]

TASK [Include dir vector role] *************************************************

TASK [vector-role : Get vector distrib rpm] ************************************
skipping: [oraclelinux]
skipping: [ubuntu]
ok: [centos7]

TASK [vector-role : Install vector rpm packages] *******************************
skipping: [oraclelinux]
skipping: [ubuntu]
ok: [centos7]

TASK [vector-role : Get vector distrib deb] ************************************
skipping: [oraclelinux]
skipping: [centos7]
ok: [ubuntu]

TASK [vector-role : Install vector deb packages] *******************************
skipping: [centos7]
skipping: [oraclelinux]
ok: [ubuntu]

TASK [vector-role : Get vector distrib rpm based for dnf packet manager] *******
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Install vector packages (dnf packet manager)] **************
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Generate vector config] ************************************
ok: [ubuntu]
ok: [centos7]
ok: [oraclelinux]

PLAY RECAP *********************************************************************
centos7                    : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
oraclelinux                : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [oraclelinux]
ok: [centos7]

TASK [Include dir vector role] *************************************************

TASK [vector-role : Get vector distrib rpm] ************************************
skipping: [oraclelinux]
skipping: [ubuntu]
ok: [centos7]

TASK [vector-role : Install vector rpm packages] *******************************
skipping: [ubuntu]
skipping: [oraclelinux]
ok: [centos7]

TASK [vector-role : Get vector distrib deb] ************************************
skipping: [centos7]
skipping: [oraclelinux]
ok: [ubuntu]

TASK [vector-role : Install vector deb packages] *******************************
skipping: [centos7]
skipping: [oraclelinux]
ok: [ubuntu]

TASK [vector-role : Get vector distrib rpm based for dnf packet manager] *******
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Install vector packages (dnf packet manager)] **************
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Generate vector config] ************************************
ok: [oraclelinux]
ok: [ubuntu]
ok: [centos7]

PLAY RECAP *********************************************************************
centos7                    : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
oraclelinux                : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

```
</details>

4. Добавьте несколько assert в verify.yml-файл для  проверки работоспособности vector-role (проверка, что конфиг валидный, проверка успешности запуска и др.).

<details>
  <summary>verify.yml</summary>

```bash
- name: Verify
  hosts: all
  gather_facts: false
  tasks:
  - name: Get Vector version
    command: vector --version
    register: vector_version_output
  - name: Read Vector config file
    slurp:
      src: "/etc/vector/vector.yaml"
    register: vector_config
  - name: Check Version
    ansible.builtin.assert:
      that:
        - vector_version_output.stdout == "vector 0.35.0 (x86_64-unknown-linux-gnu e57c0c0 2024-01-08 14:42:10.10390$      success_msg : "its all ok"
      fail_msg: "wrong version"
  - name: Check endpoin in vector.yaml
    ansible.builtin.assert:
      that:
        - "'endpoint' in vector_config['content'] | b64decode | string"
      success_msg : "its all ok"
      fail_msg: "nginxdb not found"
```
</details>

<details>
  <summary>Запуск с файлом verify.yml</summary>

```bash
[root@ansible01 vector-role]# molecule test --destroy=never
WARNING  Driver docker does not provide a schema.
INFO     default scenario test matrix: dependency, cleanup, destroy, syntax, create, prepare, converge, idempotence, side_effect, verify, cleanup, destroy
INFO     Performing prerun with role_name_check=1...
INFO     Running default > dependency
WARNING  Skipping, missing the requirements file.
WARNING  Skipping, missing the requirements file.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
WARNING  Skipping, '--destroy=never' requested.
INFO     Running default > syntax
INFO     Sanity checks: 'docker'

playbook: /root/mnt-homeworks/08-ansible-04-role/playbook/roles/vector-role/molecule/default/converge.yml
INFO     Running default > create

PLAY [Create] ******************************************************************

TASK [Set async_dir for HOME env] **********************************************
ok: [localhost]

TASK [Log into a Docker registry] **********************************************
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost] => (item=None)
skipping: [localhost]

TASK [Check presence of custom Dockerfiles] ************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})

TASK [Create Dockerfiles from image names] *************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})
skipping: [localhost]

TASK [Synchronization the context] *********************************************
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
skipping: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})
skipping: [localhost]

TASK [Discover local Docker images] ********************************************
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 0, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 1, 'ansible_index_var': 'i'})
ok: [localhost] => (item={'changed': False, 'skipped': True, 'skip_reason': 'Conditional result was False', 'false_condition': 'not item.pre_build_image | default(false)', 'item': {'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True}, 'ansible_loop_var': 'item', 'i': 2, 'ansible_index_var': 'i'})

TASK [Build an Ansible compatible image (new)] *********************************
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/centos:7)
skipping: [localhost] => (item=molecule_local/docker.io/pycontribs/ubuntu:latest)
skipping: [localhost] => (item=molecule_local/docker.io/oraclelinux:8)
skipping: [localhost]

TASK [Create docker network(s)] ************************************************
skipping: [localhost]

TASK [Determine the CMD directives] ********************************************
ok: [localhost] => (item={'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True})
ok: [localhost] => (item={'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True})

TASK [Create molecule instance(s)] *********************************************
changed: [localhost] => (item=centos7)
changed: [localhost] => (item=ubuntu)
changed: [localhost] => (item=oraclelinux)

TASK [Wait for instance(s) creation to complete] *******************************
ok: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j205001190521.9401', 'results_file': '/root/.ansible_async/j205001190521.9401', 'changed': True, 'item': {'image': 'docker.io/pycontribs/centos:7', 'name': 'centos7', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
ok: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j612476286228.9425', 'results_file': '/root/.ansible_async/j612476286228.9425', 'changed': True, 'item': {'image': 'docker.io/pycontribs/ubuntu:latest', 'name': 'ubuntu', 'pre_build_image': True}, 'ansible_loop_var': 'item'})
ok: [localhost] => (item={'failed': 0, 'started': 1, 'finished': 0, 'ansible_job_id': 'j391989112079.9456', 'results_file': '/root/.ansible_async/j391989112079.9456', 'changed': True, 'item': {'image': 'docker.io/oraclelinux:8', 'name': 'oraclelinux', 'pre_build_image': True}, 'ansible_loop_var': 'item'})

PLAY RECAP *********************************************************************
localhost                  : ok=6    changed=1    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0

INFO     Running default > prepare
WARNING  Skipping, prepare playbook not configured.
INFO     Running default > converge

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [oraclelinux]
ok: [centos7]

TASK [Include dir vector role] *************************************************

TASK [vector-role : Get vector distrib rpm] ************************************
skipping: [oraclelinux]
skipping: [ubuntu]
ok: [centos7]

TASK [vector-role : Install vector rpm packages] *******************************
skipping: [ubuntu]
skipping: [oraclelinux]
ok: [centos7]

TASK [vector-role : Get vector distrib deb] ************************************
skipping: [oraclelinux]
skipping: [centos7]
ok: [ubuntu]

TASK [vector-role : Install vector deb packages] *******************************
skipping: [centos7]
skipping: [oraclelinux]
ok: [ubuntu]

TASK [vector-role : Get vector distrib rpm based for dnf packet manager] *******
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Install vector packages (dnf packet manager)] **************
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Generate vector config] ************************************
ok: [oraclelinux]
ok: [centos7]
ok: [ubuntu]

PLAY RECAP *********************************************************************
centos7                    : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
oraclelinux                : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Running default > idempotence

PLAY [Converge] ****************************************************************

TASK [Gathering Facts] *********************************************************
ok: [ubuntu]
ok: [oraclelinux]
ok: [centos7]

TASK [Include dir vector role] *************************************************

TASK [vector-role : Get vector distrib rpm] ************************************
skipping: [ubuntu]
skipping: [oraclelinux]
ok: [centos7]

TASK [vector-role : Install vector rpm packages] *******************************
skipping: [oraclelinux]
skipping: [ubuntu]
ok: [centos7]

TASK [vector-role : Get vector distrib deb] ************************************
skipping: [centos7]
skipping: [oraclelinux]
ok: [ubuntu]

TASK [vector-role : Install vector deb packages] *******************************
skipping: [centos7]
skipping: [oraclelinux]
ok: [ubuntu]

TASK [vector-role : Get vector distrib rpm based for dnf packet manager] *******
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Install vector packages (dnf packet manager)] **************
skipping: [centos7]
skipping: [ubuntu]
ok: [oraclelinux]

TASK [vector-role : Generate vector config] ************************************
ok: [centos7]
ok: [ubuntu]
ok: [oraclelinux]

PLAY RECAP *********************************************************************
centos7                    : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
oraclelinux                : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0
ubuntu                     : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0

INFO     Idempotence completed successfully.
INFO     Running default > side_effect
WARNING  Skipping, side effect playbook not configured.
INFO     Running default > verify
INFO     Running Ansible Verifier

PLAY [Verify] ******************************************************************

TASK [Get Vector version] ******************************************************
changed: [ubuntu]
changed: [oraclelinux]
changed: [centos7]

TASK [Read Vector config file] *************************************************
ok: [centos7]
ok: [ubuntu]
ok: [oraclelinux]

TASK [Check Version] ***********************************************************
ok: [centos7] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [oraclelinux] => {
    "changed": false,
    "msg": "All assertions passed"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "All assertions passed"
}

TASK [Check endpoin in vector.yaml] ********************************************
ok: [centos7] => {
    "changed": false,
    "msg": "its all ok"
}
ok: [oraclelinux] => {
    "changed": false,
    "msg": "its all ok"
}
ok: [ubuntu] => {
    "changed": false,
    "msg": "its all ok"
}

PLAY RECAP *********************************************************************
centos7                    : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
oraclelinux                : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
ubuntu                     : ok=4    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0

INFO     Verifier completed successfully.
INFO     Running default > cleanup
WARNING  Skipping, cleanup playbook not configured.
INFO     Running default > destroy
WARNING  Skipping, '--destroy=never' requested.
```
</details>



5. Запустите тестирование роли повторно и проверьте, что оно прошло успешно.
6. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

### Tox

1. Добавьте в директорию с vector-role файлы из [директории](./example).
2. Запустите `docker run --privileged=True -v <path_to_repo>:/opt/vector-role -w /opt/vector-role -it aragast/netology:latest /bin/bash`, где path_to_repo — путь до корня репозитория с vector-role на вашей файловой системе.
3. Внутри контейнера выполните команду `tox`, посмотрите на вывод.
5. Создайте облегчённый сценарий для `molecule` с драйвером `molecule_podman`. Проверьте его на исполнимость.
6. Пропишите правильную команду в `tox.ini`, чтобы запускался облегчённый сценарий.
8. Запустите команду `tox`. Убедитесь, что всё отработало успешно.
9. Добавьте новый тег на коммит с рабочим сценарием в соответствии с семантическим версионированием.

После выполнения у вас должно получится два сценария molecule и один tox.ini файл в репозитории. Не забудьте указать в ответе теги решений Tox и Molecule заданий. В качестве решения пришлите ссылку на  ваш репозиторий и скриншоты этапов выполнения задания. 

## Необязательная часть

1. Проделайте схожие манипуляции для создания роли LightHouse.
2. Создайте сценарий внутри любой из своих ролей, который умеет поднимать весь стек при помощи всех ролей.
3. Убедитесь в работоспособности своего стека. Создайте отдельный verify.yml, который будет проверять работоспособность интеграции всех инструментов между ними.
4. Выложите свои roles в репозитории.

В качестве решения пришлите ссылки и скриншоты этапов выполнения задания.

---

### Как оформить решение задания

Выполненное домашнее задание пришлите в виде ссылки на .md-файл в вашем репозитории.
