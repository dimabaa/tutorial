# 📦 Работа с архивами в Linux: полный практический гайд

> Архивирование — одна из самых частых задач в Linux. С помощью архивов можно объединять файлы, уменьшать их размер, создавать резервные копии и удобно передавать данные между серверами.
>
> В этом руководстве разберём основные инструменты: `tar`, `gzip`, `bzip2`, `xz`, `zip` и `unzip`.

---

# 🤔 Архив и сжатие — это не одно и то же

Многие новички думают, что архив автоматически означает сжатие.

На самом деле:

- **Архивирование** — объединение нескольких файлов в один контейнер.
- **Сжатие** — уменьшение размера данных.

Например:

```text
project/
├── app.py
├── config.yaml
└── README.md
```

После архивирования:

```text
project.tar
```

После архивирования и сжатия:

```text
project.tar.gz
```

---

# 📁 Создадим тестовые файлы

Для примеров создадим небольшую структуру:

```bash
mkdir project

touch project/app.py
touch project/config.yaml
touch project/README.md
```

Проверим:

```bash
tree project
```

Результат:

```text
project
├── app.py
├── config.yaml
└── README.md
```

---

# 📦 Работа с tar

## Создание архива

Команда:

```bash
tar -cvf project.tar project/
```

Расшифровка:

| Параметр | Значение |
|-----------|-----------|
| c | создать архив |
| v | подробный вывод |
| f | имя файла архива |

После выполнения:

```text
project.tar
```

---

## Посмотреть содержимое архива

```bash
tar -tvf project.tar
```

Результат:

```text
project/
project/app.py
project/config.yaml
project/README.md
```

---

## Извлечь архив

```bash
tar -xvf project.tar
```

Параметр:

| Ключ | Значение |
|--------|----------|
| x | извлечь |

---

## Извлечь архив в другую папку

```bash
tar -xvf project.tar -C /tmp
```

---

# 🗜️ Сжатие через gzip

## Создать tar.gz

Самый популярный формат в Linux.

```bash
tar -czvf project.tar.gz project/
```

Дополнительный ключ:

| Ключ | Значение |
|--------|----------|
| z | использовать gzip |

Получаем:

```text
project.tar.gz
```

---

## Распаковать tar.gz

```bash
tar -xzvf project.tar.gz
```

---

## Сжать существующий файл

Создадим файл:

```bash
echo "Hello Linux" > test.txt
```

Сжимаем:

```bash
gzip test.txt
```

Получим:

```text
test.txt.gz
```

Исходный файл будет удалён.

---

## Распаковать gzip

```bash
gunzip test.txt.gz
```

или

```bash
gzip -d test.txt.gz
```

---

# 🗜️ Сжатие через bzip2

Алгоритм сжимает лучше gzip, но работает медленнее.

---

## Сжатие

```bash
bzip2 logfile.log
```

Результат:

```text
logfile.log.bz2
```

---

## Распаковка

```bash
bunzip2 logfile.log.bz2
```

или

```bash
bzip2 -d logfile.log.bz2
```

---

## Создание tar.bz2

```bash
tar -cjvf project.tar.bz2 project/
```

Ключ:

```text
j = bzip2
```

---

## Распаковка

```bash
tar -xjvf project.tar.bz2
```

---

# 🚀 Сжатие через xz

Один из лучших алгоритмов по степени сжатия.

Особенно популярен для больших архивов.

---

## Сжатие файла

```bash
xz backup.sql
```

Получаем:

```text
backup.sql.xz
```

---

## Распаковка

```bash
unxz backup.sql.xz
```

или

```bash
xz -d backup.sql.xz
```

---

## Создание tar.xz

```bash
tar -cJvf project.tar.xz project/
```

Ключ:

```text
J = xz
```

---

## Распаковка

```bash
tar -xJvf project.tar.xz
```

---

# 📦 Работа с ZIP

ZIP часто используется для Windows и кроссплатформенного обмена файлами.

---

## Установка

Ubuntu/Debian:

```bash
sudo apt install zip unzip
```

CentOS/RHEL:

```bash
sudo yum install zip unzip
```

---

## Создать ZIP-архив

```bash
zip project.zip app.py config.yaml README.md
```

---

## Архивировать директорию

```bash
zip -r project.zip project/
```

Ключ:

```text
r = рекурсивно
```

---

## Просмотреть содержимое

```bash
unzip -l project.zip
```

---

## Распаковать архив

```bash
unzip project.zip
```

---

## Распаковать в указанную папку

```bash
unzip project.zip -d /tmp/project
```

---

# 🔒 Архив с паролем

Создать защищённый ZIP:

```bash
zip -e backup.zip database.sql
```

Система запросит пароль:

```text
Enter password:
Verify password:
```

---

## Распаковка

```bash
unzip backup.zip
```

Потребуется пароль.

---

# 📊 Сравнение форматов

| Формат | Сжатие | Скорость | Популярность |
|----------|----------|----------|----------|
| tar | ❌ | Очень быстро | Linux |
| tar.gz | ⭐⭐⭐ | Быстро | Очень высокая |
| tar.bz2 | ⭐⭐⭐⭐ | Средне | Средняя |
| tar.xz | ⭐⭐⭐⭐⭐ | Медленно | Высокая |
| zip | ⭐⭐⭐ | Быстро | Windows/Linux |

---

# 🔍 Проверка размера архивов

Размер файла:

```bash
ls -lh
```

Пример:

```text
-rw-r--r-- 12M project.tar
-rw-r--r-- 3M project.tar.gz
-rw-r--r-- 2M project.tar.xz
```

---

# 💾 Резервное копирование

Создать архив домашней директории:

```bash
tar -czvf backup-home.tar.gz ~/Documents
```

---

## Архивировать несколько директорий

```bash
tar -czvf backup.tar.gz \
    project \
    configs \
    scripts
```

---

# 🚀 Практический пример

Предположим, нужно отправить проект коллеге.

### Создаём архив

```bash
tar -czvf project.tar.gz project/
```

Проверяем:

```bash
ls -lh project.tar.gz
```

Содержимое:

```bash
tar -tzf project.tar.gz
```

Передаём архив.

Коллега распаковывает:

```bash
tar -xzvf project.tar.gz
```

Готово.

---

# 🎯 Самые полезные команды

| Команда | Назначение |
|----------|------------|
| `tar -cvf archive.tar dir/` | Создать tar |
| `tar -xvf archive.tar` | Распаковать tar |
| `tar -czvf archive.tar.gz dir/` | Создать tar.gz |
| `tar -xzvf archive.tar.gz` | Распаковать tar.gz |
| `gzip file.txt` | Сжать файл |
| `gunzip file.txt.gz` | Распаковать |
| `zip -r archive.zip dir/` | Создать ZIP |
| `unzip archive.zip` | Распаковать ZIP |
| `unzip -l archive.zip` | Содержимое ZIP |
| `tar -tvf archive.tar` | Содержимое tar |

---

# Заключение

Для большинства задач достаточно запомнить всего несколько команд:

```bash
# Создать архив
tar -czvf archive.tar.gz folder/

# Посмотреть содержимое
tar -tzf archive.tar.gz

# Распаковать
tar -xzvf archive.tar.gz

# ZIP
zip -r archive.zip folder/
unzip archive.zip
```

Эти команды покрывают более 90% повседневной работы с архивами на Linux-серверах, VPS, Docker-хостах и рабочих станциях.
