# zabbix6


## Доступные каталоги Zabbix сервера для Docker контейненра.

|Каталог   |Описание   |
|----------|-----------|
|**/usr/lib/zabbix/alertscripts**   |Том используется для пользовательских сценариев  предупреждений. Это параметр AlertScriptsPath в zabbix_server.conf.|
|**/usr/lib/zabbix/externalscripts**|Этот каталог используется внешними проверками (тип предметов). Это параметр ExternalScripts в zabbix_server.conf.|
|**/var/lib/zabbix/modules**        |Каталог для подгрузки дополнительных модулей и расширений сервера Zabbix с помощью функции LoadModule.|
|**/var/lib/zabbix/enc**            |Том используется для хранения файлов, связанных с TLS. Эти имена файлов задаются с помощью переменных , ZBX_TLSCAFILEи ZBX_TLSCRLFILE.ZBX_TLSKEY_FILEZBX_TLSPSKFILE|
|**/var/lib/zabbix/ssh_keys**       |Том используется как местонахождение открытых и закрытых ключей для проверок и действий SSH. Это SSHKeyLocationпараметр в zabbix_server.conf.|
|**/var/lib/zabbix/ssl/certs**      | Том используется в качестве местоположения файлов сертификатов клиента SSL для аутентификации клиента. Это SSLCertLocationпараметр в zabbix_server.conf.|
|**/var/lib/zabbix/ssl/keys**       | Том используется как местонахождение файлов закрытого ключа SSL для аутентификации клиента. Это SSLKeyLocation параметр в zabbix_server.conf.|
|**/var/lib/zabbix/ssl/ssl_ca**     | Том используется в качестве расположения файлов центра сертификации (CA) для проверки сертификата сервера SSL. Это SSLCALocation параметр в zabbix_server.conf.|
|**/var/lib/zabbix/snmptraps**      | Том используется как местонахождение snmptraps.log файла. Он может использоваться совместно zabbix-snmptraps контейнером и наследоваться с помощью volumes_from опции Docker при создании нового экземпляра сервера Zabbix. Функцию обработки ловушек SNMP можно было включить, используя общий том и  переключив ZBX_ENABLE_SNMP_TRAPS переменную среды на true.|
|**/var/lib/zabbix/mibs**           | Том позволяет добавлять новые файлы MIB. Он не поддерживает подкаталоги,  все MIB должны быть помещены в /var/lib/zabbix/mibs.|
|**/var/lib/zabbix/export**         | Каталог для экспорта событий, истории и тенденций в режиме реального времени в  формате JSON с разделителями строк. Может быть включен с помощью ZBX_EXPORTFILESIZE переменной среды.|

