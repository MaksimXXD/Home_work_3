## Шаблон для третьего ДЗ по ИПР

Условие дз доступно по [ссылке](https://github.com/Nikfilk2030/missing_semester_hse_cs/blob/main/2026/amcs/year_1/hw_3/README.md)


## Отчет

### ДЗ на 4 балла:
#### Логи:
```text
2026-11-19 22:20:34,438 listening port=8000 storage=sqlite
2026-11-19 22:24:57,757 http 192.168.65.1 code 400, message Bad request version ('Uuô¡;MÖÝ\x00ú\x08ê0\x8fe\x89³B«èÛ2ÎBV\x97·\x06*tE\x8b\x00b\x13\x03\x13\x02\x13\x01Ì©Ì¨ÌªÀ0À,À(À$À\x14À')
2026-11-19 22:24:57,757 http 192.168.65.1 "($l*¥l¸A³zº¸/U3w)
                                                                    ^T
                                                                      Bj Uuô¡;MÖÝê0e³B«èÛ2ÎBV·*tEbÌ©Ì¨ÌªÀ0À,À(À$ÀÀ" 400 -
2026-11-19 22:25:12,272 http 192.168.65.1 "GET / HTTP/1.1" 200 -
2026-11-19 22:26:15,274 http 192.168.65.1 "POST /increment HTTP/1.1" 200 -
2026-11-19 22:27:22,228 http 192.168.65.1 "POST /increment HTTP/1.1" 200 -
2026-11-19 22:27:57,386 http 192.168.65.1 code 501, message Unsupported method ('-POST')
2026-11-19 22:27:57,386 http 192.168.65.1 "-POST /ready HTTP/1.1" 501 -
2026-11-19 22:28:21,430 http 192.168.65.1 code 501, message Unsupported method ('-POST')
2026-11-19 22:28:21,430 http 192.168.65.1 "-POST /readyz HTTP/1.1" 501 -
2026-11-19 22:28:52,956 http 192.168.65.1 "GET /readyz HTTP/1.1" 200 -
```

### ДЗ на 6 баллов:

Запускаем Compose и увеличиваем счётчик
```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose up -d --build
[+] up 4/4
 ✔ Image home_work_3-app       Built
 ✔ Network home_work_3_default Created
 ✔ Volume home_work_3_counter  Created
 ✔ Container home_work_3-app-1 Started
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 1, "hostname": "b426147729a8", "storage": "sqlite"}%                                                            maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 2, "hostname": "b426147729a8", "storage": "sqlite"}%
```
Удаляем compose(Видим, что именованный volume не удалился)
```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose down
[+] down 2/2
 ✔ Container home_work_3-app-1 Removed                                                                                                                                                             
 ✔ Network home_work_3_default Removed
```
Повторно запускаем Compose-проект и создаём новый контейнер. Видим что счет сохранился в counter
```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose up -d --build
[+] up 3/3
 ✔ Image home_work_3-app       Built
 ✔ Network home_work_3_default Created
  ✔ Container home_work_3-app-1 Started
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 3, "hostname": "cf8343d9f460", "storage": "sqlite"}%
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 4, "hostname": "cf8343d9f460", "storage": "sqlite"}%
```


### Дз на 8 баллов:

Запускаем Compose-проект с контейнерами app и redis

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose up -d --build
[+] up 4/5
 ✔ Image home_work_3-app         Built 
[+] up 5/5
 ✔ Volume home_work_3_counter    Created
 ⠋ Container home_work_3-redis-1 Waiting
 ⠙ Container home_work_3-redis-1 Waiting
 ✔ Container home_work_3-redis-1 Healthy
 ✔ Container home_work_3-app-1   Started
```
Проверяем работу

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose ps
NAME                  IMAGE              COMMAND                  SERVICE   CREATED          STATUS                    PORTS
home_work_3-app-1     home_work_3-app    "python app.py"          app       11 seconds ago   Up 7 seconds (healthy)    0.0.0.0:1111->8000/tcp, [::]:1111->8000/tcp
home_work_3-redis-1   redis:7.4-alpine   "docker-entrypoint.s…"   redis     11 seconds ago   Up 10 seconds (healthy)   6379/tcp
```
Увеличиваем значение(все работает)

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 1, "hostname": "1da0c1f39a3c", "storage": "redis"}%                                                                                                                 maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 2, "hostname": "1da0c1f39a3c", "storage": "redis"}%                                                                                                                 maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 3, "hostname": "1da0c1f39a3c", "storage": "redis"}%
```

Останавливаем сервис Redis

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose stop redis
[+] stop 1/1
 ✔ Container home_work_3-redis-1 Stopped
```

Контейнер все еще работает

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -i http://127.0.0.1:1111/healthz
HTTP/1.0 200 OK
Server: BaseHTTP/0.6 Python/3.13.18
Date: Sun, 22 Nov 2026 15:53:39 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 19
Cache-Control: no-store

{"status": "alive"}%
```

Нельзя взять значение и нельзя с ним ничего сделать(поскольку сервис Redis остановлен)

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -i http://127.0.0.1:1111/readyz
HTTP/1.0 503 Service Unavailable
Server: BaseHTTP/0.6 Python/3.13.18
Date: Sun, 22 Nov 2026 15:54:02 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 32
Cache-Control: no-store

{"error": "storage_unavailable"}%
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
curl: (22) The requested URL returned error: 503
```
Запускаем снова сервер, видим что все работает и значение сохранилось(то есть по сути оно хранится не внутри контейнера а во вне)

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose start redis
[+] start 1/1
 ✔ Container home_work_3-redis-1 Started                                                                                                                                                                         0.1s
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -i http://127.0.0.1:1111/healthz
HTTP/1.0 200 OK
Server: BaseHTTP/0.6 Python/3.13.18
Date: Sun, 22 Nov 2026 15:54:49 GMT
Content-Type: application/json; charset=utf-8
Content-Length: 19
Cache-Control: no-store

{"status": "alive"}%
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 4, "hostname": "1da0c1f39a3c", "storage": "redis"}%                                                                                                                 maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS -X POST http://127.0.0.1:1111/increment
{"greeting": "Привет, Docker Compose!", "count": 5, "hostname": "1da0c1f39a3c", "storage": "redis"}%
```
Видим, что после удаления контейнера и снова создания переменная осталась при своем же значение.

```text
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS http://127.0.0.1:1111/
{"greeting": "Привет, Docker Compose!", "count": 5, "hostname": "1da0c1f39a3c", "storage": "redis"}%                                                                                                                 maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose down
[+] down 3/3
 ✔ Container home_work_3-app-1   Removed
 ✔ Container home_work_3-redis-1 Removed
 ✔ Network home_work_3_default   Removed
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % docker compose up -d
[+] up 3/3
 ✔ Network home_work_3_default   Created
 ✔ Container home_work_3-redis-1 Healthy
 ✔ Container home_work_3-app-1   Started
maksimdundin@MacBook-Air-Maksim-3 Home_work_3 % curl -fsS http://127.0.0.1:1111/
{"greeting": "Привет, Docker Compose!", "count": 5, "hostname": "8d9175dca576", "storage": "redis"}%
```