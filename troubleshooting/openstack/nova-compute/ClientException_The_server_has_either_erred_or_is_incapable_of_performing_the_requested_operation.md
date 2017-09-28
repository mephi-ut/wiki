Ошибка в логе сервиса nova-compute:
```
ClientException: The server has either erred or is incapable of performing the requested operation.
```

Решение: перезапустить сервис `cinder-volume`:
```
sudo systemctl restart cinder-volume
```
