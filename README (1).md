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
<img width="906" height="216" alt="image" src="https://github.com/user-attachments/assets/f8282d71-2991-4df4-9698-478052f8e42f" />

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
<img width="974" height="651" alt="image" src="https://github.com/user-attachments/assets/31f047fd-7348-4f07-bf86-f5d2cea2c815" />

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
<img width="974" height="208" alt="image" src="https://github.com/user-attachments/assets/afed0857-e9e8-4a49-bf54-8a6d21af445f" />

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
<img width="974" height="200" alt="image" src="https://github.com/user-attachments/assets/bda226dc-3106-4616-af26-9d95c7c24d7f" />

<img width="974" height="164" alt="image" src="https://github.com/user-attachments/assets/9f734cb1-8c0c-4016-b181-3b9f305a4945" />

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
<img width="974" height="622" alt="image" src="https://github.com/user-attachments/assets/f32d669d-ca7c-41e7-b01e-2dffd4062447" />

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
<img width="974" height="730" alt="image" src="https://github.com/user-attachments/assets/f8c5b99e-9a97-4f34-ad39-40688f60f16e" />

<img width="974" height="364" alt="image" src="https://github.com/user-attachments/assets/573ed034-3af6-4002-93f3-92a87e1628ac" />

---

## Вывод

В ходе выполнения практической работы были изучены дополнительные возможности Docker по управлению жизненным циклом контейнеров, мониторингу их состояния и контролю использования системных ресурсов. Были получены практические навыки работы с логами контейнеров, инструментом docker stats, а также механизмами ограничения ресурсов по CPU и памяти. Кроме того, освоены операции экспорта и импорта контейнеров, позволяющие сохранять и восстанавливать состояние приложений. Полученные знания и навыки обеспечивают понимание операционных аспектов контейнеризации, необходимых для построения надёжных и масштабируемых приложений в production-среде.
