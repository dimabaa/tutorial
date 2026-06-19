# 🐱 Apache Tomcat: базовые принципы работы и управление сервером

> Apache Tomcat — один из самых популярных Java Servlet Container и Web Server для запуска Java-приложений. Если вы работаете с Java, Spring Boot, Jakarta EE или корпоративными приложениями, рано или поздно столкнётесь с Tomcat.

В этом руководстве разберём:
- что такое Tomcat;
- структуру каталогов;
- запуск и остановку сервера;
- деплой приложений;
- работу с логами;
- настройку портов;
- основные конфигурационные файлы.

---

# 🤔 Что такое Tomcat

Tomcat — это сервер приложений, который умеет выполнять:

- Java Servlets
- JSP (Java Server Pages)
- WebSocket
- Jakarta EE Web Profile компоненты

Схема работы:

```text
Пользователь
     │
     ▼
 HTTP Request
     │
     ▼
 Apache Tomcat
     │
     ▼
 Java Application
     │
     ▼
 HTTP Response
```

Например:

```text
http://server:8080/app
```

Запрос приходит в Tomcat, который передаёт его Java-приложению.

---

# 📦 Установка Tomcat

## Ubuntu / Debian

Установка Java:

```bash
sudo apt update
sudo apt install openjdk-17-jdk
```

Проверка:

```bash
java -version
```

Пример:

```text
openjdk version "17.0.8"
```

---

## Скачать Tomcat

```bash
wget https://downloads.apache.org/tomcat/tomcat-10/v10.x.x/bin/apache-tomcat-10.x.x.tar.gz
```

Распаковать:

```bash
tar -xzvf apache-tomcat-10.x.x.tar.gz
```

Переходим в каталог:

```bash
cd apache-tomcat-10.x.x
```

---

# 📁 Структура каталогов

После установки увидим:

```text
apache-tomcat/
├── bin
├── conf
├── lib
├── logs
├── temp
├── webapps
└── work
```

---

## bin

Содержит скрипты запуска.

```text
startup.sh
shutdown.sh
catalina.sh
```

---

## conf

Конфигурация сервера.

```text
server.xml
web.xml
tomcat-users.xml
```

---

## webapps

Каталог приложений.

```text
ROOT
manager
host-manager
docs
```

Сюда обычно копируются WAR-файлы.

---

## logs

Логи сервера.

```text
catalina.out
localhost.log
manager.log
```

---

# 🚀 Запуск сервера

## Старт

```bash
./bin/startup.sh
```

Вывод:

```text
Tomcat started.
```

---

## Проверка

```bash
ps aux | grep tomcat
```

или

```bash
ss -tulpn | grep 8080
```

---

## Открыть в браузере

```text
http://localhost:8080
```

Если всё работает:

```text
If you're seeing this, you've successfully installed Tomcat.
```

---

# 🛑 Остановка сервера

```bash
./bin/shutdown.sh
```

Проверка:

```bash
ps aux | grep tomcat
```

---

# 📦 Деплой приложения

Tomcat запускает приложения из WAR-файлов.

Например:

```text
myapp.war
```

---

## Копирование приложения

```bash
cp myapp.war webapps/
```

Tomcat автоматически распакует архив:

```text
webapps/
├── myapp.war
└── myapp/
```

---

## Доступ к приложению

```text
http://localhost:8080/myapp
```

---

# �� Структура WAR-файла

WAR похож на ZIP-архив.

Содержимое:

```text
myapp.war
├── WEB-INF
│   ├── web.xml
│   ├── classes
│   └── lib
├── index.jsp
└── static
```

---

# ⚙️ Основные порты

По умолчанию Tomcat использует:

| Порт | Назначение |
|--------|------------|
| 8080 | HTTP |
| 8005 | Shutdown |
| 8009 | AJP |

---

## Проверка портов

```bash
ss -tulpn | grep java
```

---

# 🔧 Изменение порта

Открываем:

```bash
nano conf/server.xml
```

Ищем:

```xml
<Connector port="8080"
           protocol="HTTP/1.1"/>
```

Меняем:

```xml
<Connector port="9090"
           protocol="HTTP/1.1"/>
```

После перезапуска:

```text
http://localhost:9090
```

---

# 📜 Работа с логами

Самый важный лог:

```bash
logs/catalina.out
```

Просмотр:

```bash
tail -f logs/catalina.out
```

---

## Смотреть ошибки в реальном времени

```bash
tail -f logs/catalina.out
```

Пример:

```text
INFO: Server startup in 1250 ms
```

Ошибка:

```text
SEVERE: Failed to start component
```

---

# 👥 Пользователи Tomcat Manager

Файл:

```bash
conf/tomcat-users.xml
```

Добавляем:

```xml
<role rolename="manager-gui"/>

<user username="admin"
      password="admin123"
      roles="manager-gui"/>
```

---

После перезапуска доступен интерфейс:

```text
http://localhost:8080/manager/html
```

---

# �� Tomcat Manager

Позволяет:

- загружать WAR-файлы;
- перезапускать приложения;
- останавливать приложения;
- смотреть состояние сервера.

---

# 🔥 Перезапуск приложения

Удаляем старую версию:

```bash
rm -rf webapps/myapp
rm -f webapps/myapp.war
```

Копируем новую:

```bash
cp myapp.war webapps/
```

Tomcat автоматически развернёт приложение.

---

# 📊 Мониторинг ресурсов

Память JVM:

```bash
ps aux | grep java
```

---

Использование памяти:

```bash
jcmd <PID> VM.info
```

или

```bash
jstat -gc <PID>
```

---

# ⚡ Настройка памяти JVM

Файл:

```bash
bin/setenv.sh
```

Создаём:

```bash
nano bin/setenv.sh
```

Содержимое:

```bash
export CATALINA_OPTS="-Xms512m -Xmx2048m"
```

Где:

| Параметр | Значение |
|-----------|----------|
| Xms | начальный объём памяти |
| Xmx | максимальный объём памяти |

---

# 🔒 Автозапуск через systemd

Пример управления сервисом:

```bash
sudo systemctl start tomcat
```

Статус:

```bash
sudo systemctl status tomcat
```

Перезапуск:

```bash
sudo systemctl restart tomcat
```

Автозагрузка:

```bash
sudo systemctl enable tomcat
```

---

# 🚀 Практический сценарий

Предположим, разработчик передал:

```text
crm.war
```

Разворачиваем:

```bash
cp crm.war /opt/tomcat/webapps/
```

Смотрим логи:

```bash
tail -f logs/catalina.out
```

Убеждаемся, что приложение поднялось:

```text
Deployment of web application archive completed
```

Проверяем:

```text
http://server:8080/crm
```

Приложение доступно.

---

# 🎯 Шпаргалка администратора

| Команда | Назначение |
|----------|------------|
| `./startup.sh` | Запуск Tomcat |
| `./shutdown.sh` | Остановка Tomcat |
| `tail -f logs/catalina.out` | Смотреть логи |
| `cp app.war webapps/` | Деплой приложения |
| `ss -tulpn | grep 8080` | Проверка порта |
| `systemctl restart tomcat` | Перезапуск сервиса |
| `java -version` | Проверка Java |
| `ps aux | grep tomcat` | Проверка процесса |

---

# Заключение

Для повседневной работы с Tomcat достаточно понимать несколько ключевых вещей:

```bash
# Запуск
./bin/startup.sh

# Остановка
./bin/shutdown.sh

# Логи
tail -f logs/catalina.out

# Деплой
cp app.war webapps/

# Проверка
http://server:8080/app
```

Освоив эти базовые операции, вы сможете самостоятельно разворачивать, обновлять и поддерживать большинство Java-приложений, работающих под Apache Tomcat.
