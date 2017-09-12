Не стартует Openstack Dashboard (Horizon).

Добавить в /etc/apache2/apache2.conf строку:

```
WSGIApplicationGroup %{GLOBAL}
```
