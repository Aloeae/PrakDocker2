# Практическая работа: Дополнительные возможности Docker

## Цель работы

Изучение дополнительных возможностей Docker для управления жизненным циклом контейнеров, мониторинга их состояния и оптимизации использования системных ресурсов. Данная практика направлена на глубокое понимание операционных аспектов контейнеризации, что критически важно для построения надежных и масштабируемых приложений в production-среде.

---

## Ход работы

### Подготовка окружения

```bash
# Создание директории для практики
mkdir docker-practice
cd docker-practice

# Копирование Dockerfile и entrypoint.sh в директорию
# (файлы должны быть созданы согласно примерам выше)

# Сборка образа
docker build -t practice-image:1.0 .
```
<img width="709" height="51" alt="Снимок экрана 2025-12-26 095142" src="https://github.com/user-attachments/assets/f4771fda-2020-493c-9ee0-1b7f77eca08f" />


### Задание 1: Вывод логов в файл

```bash
# Запуск контейнера
docker run -d --name practice-container-1 practice-image:1.0 30

# Ожидание завершения контейнера
sleep 35

# Сохранение логов
docker logs practice-container-1 > /tmp/container_logs.txt

# Просмотр логов
cat /tmp/container_logs.txt

# Очистка
docker rm practice-container-1
```
<img width="1690" height="814" alt="Снимок экрана 2025-12-26 095917" src="https://github.com/user-attachments/assets/6ccc3079-a875-42b8-8b37-f397d9c00a29" />

### Задание 2: Проверка docker-stats

```bash
# Запуск контейнера в background
docker run -d --name practice-container-2 practice-image:1.0 45

# В другом терминале: просмотр статистики в реальном времени
docker stats practice-container-2

# Или сохранение одного снимка статистики
docker stats --no-stream practice-container-2 > /tmp/container_stats.txt

# После завершения контейнера
docker rm practice-container-2
```
<img width="1718" height="159" alt="Снимок экрана 2025-12-26 100038" src="https://github.com/user-attachments/assets/d108ddc9-64fa-4667-bc8c-3283eb0bbaff" />


### Задание 3: Ограничение ресурсов

```bash
# Запуск контейнера с ограничением памяти и CPU
docker run -d --name practice-limited \
  --memory=256m \
  --cpus=0.5 \
  practice-image:1.0 60

# Проверка статистики с учетом лимитов
docker stats --no-stream practice-limited

# Обновление лимитов на работающем контейнере
docker update --memory=512m practice-limited

# Очистка
docker stop practice-limited
docker rm practice-limited
```
<img width="1574" height="397" alt="Снимок экрана 2025-12-26 100255" src="https://github.com/user-attachments/assets/cdac469f-d743-4cd0-ba8d-f85c5324c36d" />

### Задание 4: Экспорт в tar

```bash
# Запуск контейнера и ожидание его завершения
docker run -d --name practice-export practice-image:1.0 30

# Ожидание завершения
sleep 35

# Экспорт контейнера в tar
docker export practice-export > /tmp/container_export.tar

# Проверка размера архива
ls -lh /tmp/container_export.tar

# Просмотр содержимого архива
tar -tf /tmp/container_export.tar | head -20

# Очистка
docker rm practice-export
```
<img width="1606" height="821" alt="Снимок экрана 2025-12-26 100720" src="https://github.com/user-attachments/assets/4fbd9623-f8ef-405d-962d-4fbf75bf15e8" />


### Задание 5: Импорт из tar

```bash
# Загрузка образа из архива
docker import /tmp/container_export.tar restored-practice:1.0

# Проверка наличия образа
docker images | grep restored

# Запуск контейнера из загруженного образа
docker run -d --name restored-from-tar restored-practice:1.0 /home/appuser/entrypoint.sh

# Проверка логов восстановленного контейнера
sleep 35
docker logs restored-from-tar

# Очистка
docker stop restored-from-tar
docker rm restored-from-tar
docker rmi restored-practice:1.0
```
<img width="1220" height="939" alt="Снимок экрана 2025-12-26 101025" src="https://github.com/user-attachments/assets/52144474-1388-4606-b136-4dbcc6b7ce34" />


---

## Вывод

В ходе выполнения практической работы были изучены дополнительные возможности Docker по управлению жизненным циклом контейнеров, мониторингу их состояния и контролю использования системных ресурсов. Были получены практические навыки работы с логами контейнеров, инструментом docker stats, а также механизмами ограничения ресурсов по CPU и памяти. Кроме того, освоены операции экспорта и импорта контейнеров, позволяющие сохранять и восстанавливать состояние приложений. Полученные знания и навыки обеспечивают понимание операционных аспектов контейнеризации, необходимых для построения надёжных и масштабируемых приложений в production-среде.
