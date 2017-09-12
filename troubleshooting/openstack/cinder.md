В релизе Pike любой запрос к cinder'у с помощью `openstack volume some-subcommand` возвращает `Internal Server Error (HTTP 500)`

В логе /var/log/apache2/cinder_error.log ошибка:
`ConfigNotFound: Could not find config at api-paste.ini`

Проблема в правах, с которыми запускается cinder-wsgi.

Решение:
- Ensure that the cinder configuration folder is owned by the 'cinder' group:
```
chown root:cinder /etc/cinder
```
- Modify the apache configuration and ensure that the process is executed using the 'cinder' group.
```
WSGIDaemonProcess cinder-wsgi processes=5 threads=1 user=cinder group=cinder display-name=%{GROUP}
```
- Restart Apache:
```
service apache2 restart