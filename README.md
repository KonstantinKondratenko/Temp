# Задача не решена
## Установка
Нет доступа к OSLL/proctoring-ml  ->  перешел к шагу "This repo"

Клонирование и билд проекта прошли успешно - логи терминала (terminal_launch_logs) прикладываю в папку logs. Как видно ошибок не возникало. Возникли предупреждения, связанные с urllib3, но они, видимо, не критичные.

После сборки проекта поднялось 7 контейнеров - результат команды `docker ps -a` помещен в папку logs (doc_ps_after_install):
1) proctoring_frontend
2) proctoring_coturn
3) proctoring_backend_image
4) chargosudar/xqueue-custom:latest
5) proctoring_kurento
6) portainer/portainer-ce:2.0.1
7) mongo:5.0
По названия контейнеров можно предположить, что база данных живет в контейнере **`mongo:5.0`**

После по инструкции выполнены команды `cd backend && npm install` и `npm start` - результат помещен в папку logs (backend_npm_install_log)

После была попытка выполнить команду `cd frontend && npm install` но появилось множесто ошибок - результат помещен в папку logs (frontend_npm_install_log)

Скрипты из папки demo не выполняются - получены ошибки `curl: (6) Could not resolve host: service` и  `curl: (6) Could not resolve host: ml`

## Исследование контейнера Mongo

При вводе команды `docker ps -a` получаем вывод, содержащий:

1) cfd1295520dd   portainer/portainer-ce:2.0.1       "/portainer --admin-…"   About an hour ago   Up About an hour             8000/tcp, 0.0.0.0:9000->9000/tcp, :::9000->9000/tcp   portainer

2) 2664b2a207f0   mongo:5.0                          "docker-entrypoint.s…"   About an hour ago   Up About an hour                                                                   proctoring_mongo

### Подключение к http://localhost/
1) при подключении к http://localhost:8000/ появляется окно регистрации:
  
 <img src="/images/port:8000.png" width="1000">

4) при подключении к http://localhost:9000/ появляется окно регистрации:

 <img src="/images/port:9000.png" width="1000">

### Исследование контейнера proctoring_mongo
1) войдем внутрь контейнера с помощью `docker exec -it 2664b2a207f0 bash`
2) выполним команду `cd && ls -lRa` - результат помещен в папку logs (docker_mongo_ls-lRa)
   
