# MQTT для бризера Ballu ONEAIR ASP-200
*За основу взят [Ballu_ASP-100](https://github.com/v-vadim/Ballu_ASP-100)*

## Добавлено для ASP-200 (в ASP-100 нет такого физически):
 * Переключение режима заслонки
 * Переключение режима U/V лампы для очистки воздуха
 * Переключение режима ионизации для очистки воздуха
---
Данный конфиг использует Packages https://www.home-assistant.io/docs/configuration/packages/

Для работы с бризером необходимо установить и настроить MQTT брокер EMQX или Mosquitto 1.5.11 (Не работает с mosquitto выше 1.5.11)
В MQTT брокере необходимо включить и настроить поддержку SSL на порту 8883 (Сертификат можно любой, бризер не проверяет сертификат сервера mqtt) Включить авторизацию по логину-паролю и добавить пользователя rusclimate с паролем указанным в конфиге (Логин и пароль зашиты в прошивку бризера и являются одинаковыми для всех устройств)

На вашем роутере необходимо настроить DNS запись mqtt.cloud.rusklimat.ru c IP вашего MQTT, после этого на несколько секунд отключить питание бризера для сброса кеша DNS.

Например на роутере Asus с прошивкой Merlin это делается через ssh + dns выдавать самого роутера
```
nano /jffs/configs/hosts.add
192.168.0.123 mqtt.cloud.rusklimat.ru
```

Подключиться к вашему MQTT серверу mqtt explorer'ом и найти топик `rusclimate/<device_type>/<device_id>/`

Заменить в конфиге [ballu_asp-200.yaml](/ballu_asp_200.yaml) **<device_type>/<device_id>** и **< MAC>** на данные вашего устройства которые отображаются в MQTT Explorer.

Пример: 
```yaml
command_topic: rusclimate/59/c9b7536b6fe724cababcf9ca3add8/control/
```

MAC пишется слитно, без разделителей
```yaml
identifiers: 64b1045c5316
```

Поместить полученный конфиг в папку **packages** вашего HA

Не забыть активировать загрузку packages в **configuration.yaml** вашего HA
```yaml
homeassistant:
  packages: !include_dir_merge_named packages/
```