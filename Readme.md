Playbook применяется на хосты, расположенные в файле inventory/prod.yml. В нем описаны хосты Elasticsearch и Kibana.

В каталоге group_vars расположены каталоги с переменными, которые соответствют хостам, описанным в файле prod.yml соответственно.

В каталоге group_vars/elasticsearch располагаются переменные для хоста elasticsearch.

В каталоге group_vars/kibana располагаются переменные для хоста kibana.

В каталоге group_vars/all располагаются переменные для всех хостов.

В каталоге files располагаются файлы, которые используются в процессе выполнения playbook.

В каталоге templates расположены шаблоны в формате Jinja, которые используются в процессе выполнения playbook для копирования в конечные хосты в целеные каталоги, при этом при копировании в шаблонах указанные переменные заменяются реальными значениями, которые содержатся в этих переменных на момент выполнения задачи с распространением файла шаблона.

Playbook состоит из 3 play:
1. Установка java на все хосты, которые описаны в файле prod.yml, каждое задание в этом play имеет tag: java:
   1. происходит опреледениее переменной java_home
   2. происхолдит копирование архива java версии, указанной в файле group_vars/all/vars.yml, из каталога files на каждый из хостов из файла prod.yml в каталог /tmp
   3. происходит проверка существования каталога по пути, который указан в переменной java_home, если каталог существует, то шаг пропускается, в противном случае происходит его создание
   4. происходит проверка существования файла <каталог java_home>/bin/java, если файл не существует, то происходит извлечение скопированного архива в каталог java_home с привелигерованными правами
   5. происходит генерация файла jdk.sh путем копирования его из файла jdk.sh.j2 из каталога templates, который является шаблоном, заменой в нем переменных на конечные значения, которые в них содержатся, в каталог /etc/profile.d
2. Установка Elasticsearch на хост elasticsearch, который описан в файле prod.yml, каждое задание в этом play имеет tag: elastic:
   1. происхолдит скачивание архива Elasticsearch версии, указанной в файле group_vars/elasticsearch/vars.yml, с официального сайта на хост elasticsearch из файла prod.yml в каталог /tmp
   2. происходит проверка существования каталога по пути, который указан в переменной elastic_home, если каталог существует, то шаг пропускается, в противном случае происходит его создание
   3. происходит проверка существования файла <каталог elastic_home>/bin/elasticsearch, если файл не существует, то происходит извлечение скопированного архива в каталог elastic_home с привелигерованными правами
   4. происходит генерация файла elk.sh путем копирования его из файла elk.sh.j2 из каталога templates, который является шаблоном, заменой в нем переменных на конечные значения, которые в них содержатся, в каталог /etc/profile.d
3. Установка Kibana на хост kibana, который описан в файле prod.yml, каждое задание в этом play имеет tag: kibana:
   1. происхолдит скачивание архива Kibana версии, указанной в файле group_vars/kibana/vars.yml, с официального сайта на хост kibana из файла prod.yml в каталог /tmp
   2. происходит проверка существования каталога по пути, который указан в переменной elastic_home, если каталог существует, то шаг пропускается, в противном случае происходит его создание
   3. происходит проверка существования файла <каталог kibana_home>/bin/kibana, если файл не существует, то происходит извлечение скопированного архива в каталог kibana_home с привелигерованными правами
   4. происходит генерация файла kibana.sh путем копирования его из файла kibana.sh.j2 из каталога templates, который является шаблоном, заменой в нем переменных на конечные значения, которые в них содержатся, в каталог /etc/profile.d

