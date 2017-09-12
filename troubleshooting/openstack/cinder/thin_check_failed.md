Ошибка про создании тома:

```
/usr/sbin/thin_check: execvp failed: No such file or directory\n  Check of pool cinder-volumes/cinder-volumes-pool failed (status:2). Manual repair required!\n'
```

Решение:

Необходимо установить пакет `thin-provisioning-tools`:

```
apt install thin-provisioning-tools
```