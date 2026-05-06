# Practical lesson pz-MQTT

## Тема роботи

Розгортання та налаштування MQTT-брокера Mosquitto у Docker-середовищі.

---

# Мета роботи

Отримати практичні навички:

* роботи з MQTT-протоколом
* розгортання MQTT-брокера
* використання Docker Compose
* виконання Publish/Subscribe
* тестування MQTT-з’єднання

---

# Що таке MQTT

MQTT (Message Queuing Telemetry Transport) — легкий мережевий протокол обміну повідомленнями між пристроями.

MQTT часто використовується:

* IoT
* Smart Home
* сенсори
* телеметрія
* системи моніторингу

---

# Основні поняття MQTT

## Broker

MQTT Broker — сервер-посередник, який приймає повідомлення від Publisher та передає їх Subscriber.

У роботі використовується:

```txt
Eclipse Mosquitto
```

---

## Topic

Topic — канал передачі повідомлень.

Приклад:

```txt
test/message
```

---

## Publish

Publish — відправлення повідомлення у Topic.

Приклад:

```txt
Hello MQTT
```

---

## Subscribe

Subscribe — підписка на Topic для отримання повідомлень.

---

## QoS

QoS (Quality of Service) — рівень гарантії доставки повідомлень.

| QoS | Опис                                                   |
| --- | ------------------------------------------------------ |
| 0   | повідомлення може бути втрачено                        |
| 1   | повідомлення гарантовано доставляється хоча б один раз |
| 2   | повідомлення гарантовано доставляється лише один раз   |

---

# Структура проєкту

```txt
pz-MQTT/
│
├── broker/
│   ├── docker-compose.yml
│   ├── mosquitto.conf
│
├── screenshots/
│
└── README.md
```

---

# Docker Compose

Файл:

```txt
broker/docker-compose.yml
```

Вміст:

```yaml
services:
  mosquitto:
    image: eclipse-mosquitto
    container_name: mqtt-broker
    ports:
      - "1883:1883"
    volumes:
      - ./mosquitto.conf:/mosquitto/config/mosquitto.conf
```

---

# Пояснення docker-compose.yml

| Параметр       | Значення                 |
| -------------- | ------------------------ |
| image          | Docker image MQTT broker |
| container_name | ім’я контейнера          |
| ports          | проброс порту 1883       |
| volumes        | підключення конфігурації |

---

# Конфігурація Mosquitto

Файл:

```txt
broker/mosquitto.conf
```

Вміст:

```conf
listener 1883
allow_anonymous true
```

---

# Пояснення конфігурації

| Команда              | Значення                              |
| -------------------- | ------------------------------------- |
| listener 1883        | запуск MQTT на порту 1883             |
| allow_anonymous true | дозволити підключення без авторизації |

---

# Запуск MQTT Broker

Перехід у директорію:

```bash
cd broker
```

Запуск Docker Compose:

```bash
docker compose up
```

---

# Що відбувається після запуску

Docker:

* завантажує image Mosquitto
* створює container
* запускає MQTT broker
* відкриває порт 1883

У логах з’являється:

```txt
mosquitto version 2.1.2 running
Opening ipv4 listen socket on port 1883
```

Це означає що MQTT broker працює коректно.

---

# Перевірка контейнерів Docker

Команда:

```bash
docker ps
```

Показує активні Docker-контейнери.

---

# Subscribe

Команда:

```bash
docker exec -it mqtt-broker mosquitto_sub -h localhost -t test/message
```

---

# Пояснення команди Subscribe

| Частина         | Значення                              |
| --------------- | ------------------------------------- |
| docker exec     | виконати команду всередині контейнера |
| -it             | інтерактивний режим                   |
| mqtt-broker     | ім’я контейнера                       |
| mosquitto_sub   | MQTT Subscriber                       |
| -h localhost    | підключення до локального broker      |
| -t test/message | Topic                                 |

Команда переходить у режим очікування повідомлень.

---

# Publish

Команда:

```bash
docker exec -it mqtt-broker mosquitto_pub -h localhost -t test/message -m "Hello MQTT"
```

---

# Пояснення команди Publish

| Частина       | Значення           |
| ------------- | ------------------ |
| mosquitto_pub | MQTT Publisher     |
| -m            | повідомлення       |
| Hello MQTT    | текст повідомлення |

---

# Результат Publish/Subscribe

Після виконання Publish:

```txt
Hello MQTT
```

повідомлення з’являється у Subscriber.

Це підтверджує працездатність MQTT broker.

---

# Основні Docker-команди

## Список контейнерів

```bash
docker ps
```

---

## Зупинка контейнера

```bash
docker stop mqtt-broker
```

---

## Видалення контейнера

```bash
docker rm mqtt-broker
```

---

## Повторний запуск

```bash
docker compose up
```

---

# Висновок

У ході практичної роботи було:

* розгорнуто MQTT broker Mosquitto
* налаштовано базову конфігурацію
* виконано Publish/Subscribe
* протестовано MQTT-протокол
* отримано практичні навички роботи з Docker та MQTT
