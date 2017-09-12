В релизе Pike любой запрос к cinder'у с помощью `openstack volume some-subcommand` возвращает `Internal Server Error (HTTP 500)`

В логе /var/log/apache2/cinder_error.log ошибка:
`ConfigNotFound: Could not find config at api-paste.ini`

Проблема в правах, с которыми запускается cinder-wsgi.

Решение:
- Проверить, что директория с конфигурационными файлами cinder'а принадлежить группе 'cinder'. Если это не так, то выполнить команду:

```
chown root:cinder /etc/cinder
```

- Изменить в конфигурационном файле /etc/apache2/conf-available/cinder-wsgi.conf строку запуска. Добавить `group=cinder`

```
WSGIDaemonProcess cinder-wsgi processes=5 threads=1 user=cinder group=cinder display-name=%{GROUP}
```

- Перезапустить Apache:

```
service apache2 restart
```

Спасибо Christoph Fiehe с Launchpad'а.
Ссылка на баг: https://bugs.launchpad.net/cinder/+bug/1715024