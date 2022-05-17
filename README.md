# zabbix6


Послеперврначальной установки:
~~~
Username: Admin
Password: zabbix
~~~


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




# Переменные среды

Когда вы запускаете zabbix-server-mysqlобраз, вы можете настроить конфигурацию сервера Zabbix, передав одну или несколько переменных среды в docker runкомандной строке.


**DB_SERVER_HOST**
Эта переменная представляет собой IP- или DNS-имя сервера MySQL. По умолчанию значение равно 'mysql-server'.

**DB_SERVER_PORT**
Эта переменная является портом сервера MySQL. По умолчанию значение равно «3306».

**MYSQL_USER**, **MYSQL_PASSWORD**, **MYSQL_USER_FILE**,**MYSQL_PASSWORD_FILE**
Эти переменные используются сервером Zabbix для подключения к базе данных Zabbix. С помощью _FILEпеременных вы можете вместо этого указать путь к файлу, который вместо этого содержит пользователя/пароль. Без Docker Swarm или Kubernetes вам также придется сопоставлять файлы. Они являются эксклюзивными, поэтому вы можете указать только один тип — либо MYSQL_USERили MYSQL_USER_FILE!

~~~
docker run --name some-zabbix-server-mysql -e DB_SERVER_HOST="some-mysql-server" -v ./.MYSQL_USER:/run/secrets/MYSQL_USER -e MYSQL_USER_FILE=/run/secrets/MYSQL_USER -v ./.MYSQL_PASSWORD:/run/secrets/MYSQL_PASSWORD -e MYSQL_PASSWORD_FILE=/var/run/secrets/MYSQL_PASSWORD -d zabbix/zabbix-server-mysql:tag
~~~

С Docker Swarm или Kubernetes это работает с секретами. Таким образом, он реплицируется в вашем кластере!

printf "zabbix" | docker secret create MYSQL_USER -
printf "zabbix" | docker secret create MYSQL_PASSWORD -
docker run --name some-zabbix-server-mysql -e DB_SERVER_HOST="some-mysql-server" -e MYSQL_USER_FILE=/run/secrets/MYSQL_USER -e MYSQL_PASSWORD_FILE=/run/secrets/MYSQL_PASSWORD -d zabbix/zabbix-server-mysql:tag

Этот метод также применим для MYSQL_ROOT_PASSWORDс MYSQL_ROOT_PASSWORD_FILE.

По умолчанию значения для MYSQL_USERи MYSQL_PASSWORDравны zabbix, zabbix.

**MYSQL_DATABASE**
Переменная — это имя базы данных Zabbix. По умолчанию значение равно zabbix.

**ZBX_LOADMODULE**
Переменная представляет собой список загружаемых модулей Zabbix, разделенных запятыми. Работает с объемом /var/lib/zabbix/modules. Синтаксис переменной такой dummy1.so,dummy2.so.

**ZBX_DEBUGLEVEL**
Переменная используется для указания уровня отладки. По умолчанию значение равно 3. Это DebugLevelпараметр в zabbix_server.conf. Допустимые значения перечислены ниже:

    0- основная информация о запуске и остановке процессов Zabbix;
    1- критическая информация
    2- информация об ошибке
    3- предупреждения
    4- для отладки (выдает много информации)
    5- расширенная отладка (выдает еще больше информации)

**ZBX_TIMEOUT**
Переменная используется для указания тайм-аута для обработки проверок. По умолчанию значение равно 4.
ZBX_JAVAGATEWAY_ENABLE

Эта переменная обеспечивает связь с Zabbix Java Gateway для сбора проверок, связанных с Java. По умолчанию значение равно false.