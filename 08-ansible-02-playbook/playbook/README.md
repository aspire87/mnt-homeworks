## Установка Clickhouse и Vector

Decription

Данный [playbook](site.yml) предназначен  для  установки `Clickhouse`  и `Vector` на хосты,  указанные в файле  `inventory`. Плейбук состоит из  двух play:
   * `Install Clickhouse` - устанавливает,  настраивает и запускает `clickhouse` на группе хостов `clickhouse`;
   * `Install Vector` -  устанавливает , настраивает и запускает `Vector`  на группе хостов `vector`



Play "Install Clickhouse":
 - Загружает дистрибутивы `Clickhouse` с версией, указанной в group_vars/clickhouse/vars.yml
 - Устанавливает пакеты clickhouse на группу хостов clickhouse
 - Запускает clickhouse-server
 - Создает БД logs для  хранения собираемых логов
 - Устанавливается vector на основе шаблонов
 - Запускается vector


Play "Install Vector":
 - Загружает дистрибутив `Vector` с версией, указанной в group_vars/vector/vars.yml
 - Устанавливает пакет  Vector на группу хостов  vector
 - Настраивает vector с помощью конфигурационного файла
 - Запускает  vector на группе хостов

Пример запуска playbook-а

```bash
$ ansible-playbook -i inventory/prod.yml site.yml
```