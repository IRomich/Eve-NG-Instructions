# Eve-NG-Instructions
## Для вайпа нод в лабе.
### В файле /opt/unetlab/html/includes/api_nodes.php в функцию apiWipeLabNode (после строки function apiWipeLabNode($lab, $id, $tenant) {) добавить
```
$cmd = 'for item in $(mount | grep \'/tmp/'.$tenant.'/'.$lab->getId().'/'.$id.'\' | awk \'{print $3}\'); do sudo umount $item; done';
exec($cmd, $o, $rc);
```
### В начало функции apiWipeLabNodes (после строки function apiWipeLabNodes($lab, $tenant) {) добавить
```
$cmd = 'for item in $(mount | grep \'/tmp/'.$tenant.'/'.$lab->getId().'/\' | awk \'{print $3}\'); do sudo umount $item; done';
exec($cmd, $o, $rc);
```

### Заменить содержимое файла /etc/sudoers.d/unetlab на 
```Defaults:apache runas_default=root, !requiretty, setenv, umask_override, umask=0002
www-data ALL=(root) NOPASSWD: /opt/unetlab/wrappers/unl_wrapper,/usr/bin/umount
```

## Для запуска нод в лабе.
### Заменить папку wrappers на содержимое архива:
```
mv wrappers.tar.gz /opt/unetlab/
cd /opt/unetlab/
mv wrappers wrappers_bak
tar -xvf wrappers.tar.gz
```
