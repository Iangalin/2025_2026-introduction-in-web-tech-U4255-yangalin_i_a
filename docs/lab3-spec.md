# Лабораторная работа №3

## "Мониторинг с Prometheus и Grafana"
### Описание
Это третья лабораторная работа по настройке системы мониторинга с использованием Prometheus для сбора метрик и Grafana для визуализации данных.

### Цель работы
Научиться настраивать локальную систему мониторинга, собирать метрики с помощью Prometheus и создавать дашборды в Grafana для визуализации данных.

### Правила по оформлению
Правила по оформлению отчета по лабораторной работе вы можете изучить по [ссылке](../reportdesign.md)

### Ход работы

#### Обычная лабораторная работа

**Настройка мониторинга с Prometheus и Grafana:**

1. **Создание конфигурации Prometheus:**
      - Создать папку `prometheus` для конфигурации
      - Создать файл `prometheus/prometheus.yml` со следующим содержимым:
            global:
                  scrape_interval: 15s
            
            scrape_configs:
                  - job_name: 'prometheus'
                  static_configs:
                  - targets: ['localhost:9090']
            
                  - job_name: 'node-exporter'
                  static_configs:
                  - targets: ['node-exporter:9100']

2. **Запуск Node Exporter:**
      - Запустить контейнер Node Exporter для сбора системных метрик:

            docker run -d \
                  --name node-exporter \
                  --restart=unless-stopped \
                  -p 9100:9100 \
                  -v "/proc:/host/proc:ro" \
                  -v "/sys:/host/sys:ro" \
                  -v "/:/rootfs:ro" \
                  prom/node-exporter \
                  --path.procfs=/host/proc \
                  --path.rootfs=/rootfs \
                  --path.sysfs=/host/sys \
                  --collector.filesystem.mount-points-exclude="^/(sys|proc|dev|host|etc)($$|/)"

      - Проверить работу: `curl http://localhost:9100/metrics`

3. **Запуск Prometheus:**
      - Создать том для данных Prometheus:

            docker volume create prometheus-data
        
      - Prometheus - приложение, которое отвечает за сбор метрик. Для их визуализации позже будет запущено другое приложение - Grafana. Чтобы они могли работать вместе, нужно создать для них общую сеть:
        
            docker network create monitoring
        
      - Выйти на папку выше в консоли, убедиться что вы находитесь над уровнем папки prometheus. Запустить контейнер Prometheus:

            docker run -d \
                  --name prometheus \
                  --network monitoring \
                  --restart=unless-stopped \
                  -p 9090:9090 \
                  -v prometheus-data:/prometheus \
                  -v $(pwd)/prometheus:/etc/prometheus \
                  prom/prometheus \
                  --config.file=/etc/prometheus/prometheus.yml \
                  --storage.tsdb.path=/prometheus \
                  --web.console.libraries=/etc/prometheus/console_libraries \
                  --web.console.templates=/etc/prometheus/consoles \
                  --storage.tsdb.retention.time=200h \
                  --web.enable-lifecycle

      - Проверить работу: открыть `http://localhost:9090` в браузере
      - В случае неполадок можно запустить команду docker logs prometheus. Там будет выведена ошибка, указывающая на причину неработающего контейнера. В случае обнаружения подобной ошибки рекомендуется попробовать починить ее самостоятельно.

5. **Запуск Grafana:**
      - Создать том для данных Grafana:

            docker volume create grafana-data

      - Запустить контейнер Grafana:

            docker run -d \
                  --name grafana \
                  --network monitoring \
                  --restart=unless-stopped \
                  -p 3000:3000 \
                  -v grafana-data:/var/lib/grafana \
                  -e "GF_SECURITY_ADMIN_PASSWORD=admin" \
                  grafana/grafana

      - Проверить работу: открыть `http://localhost:3000` в браузере (логин: admin, пароль: admin)

6. **Настройка Grafana:**
      - Войти в Grafana (admin/admin)
      - Добавить источник данных Prometheus:
        - Configuration → Data Sources → Add data source
        - Выбрать Prometheus
        - URL: `http://prometheus:9090`
        - Save & Test
      - Создать дашборд:
        - Create → Dashboard → Add visualization
        - Выбрать источник данных Prometheus
        - Добавить метрику: `node_cpu_seconds_total`
        - Сохранить дашборд

7. **Тестирование системы:**
      - Проверить все контейнеры: `docker ps`
      - Открыть Prometheus и убедиться, что метрики собираются
      - Открыть Grafana и проверить отображение графиков
      - Создать несколько графиков для разных метрик (CPU, память, диск)

#### Лабораторная работа со звездочкой

**Тестирование безопасности веб-сайтов:**

1. **Выбор цели для тестирования:**
      - Выбрать доступный сайт для тестирования (рекомендуется не очень популярные сайты)
      - Примеры подходящих сайтов: сайты местных организаций, спортивных федераций, небольших компаний
      - НЕ выбирать: Google, Яндекс, крупные банки, государственные сайты
      - Записать URL выбранного сайта в отчет

2. **Подготовка инструментов:**
      - Установить ffuf (если нет): `go install github.com/ffuf/ffuf@latest`
      - Подготовить wordlist для перебора (можно использовать стандартные)
      - Настроить браузер для перехвата запросов (опционально)

3. **Тестирование Path Traversal:**
      - Проверить уязвимость path traversal на различных эндпоинтах
      - Попробовать запросы типа:
        - `http://example.com/../../../etc/passwd`
        - `http://example.com/..%2F..%2F..%2Fetc%2Fpasswd`
        - `http://example.com/....//....//....//etc/passwd`
      - Проверить разные кодировки и варианты обхода фильтров
      - Записать все попытки и результаты в отчет

4. **Перебор страниц через ffuf:**
      - Запустить ffuf для поиска скрытых директорий:


            ffuf -w /path/to/wordlist.txt -u http://example.com/FUZZ -mc 200,301,302,403


      - Попробовать разные wordlist'ы (common.txt, directory-list-2.3-medium.txt)
      - Настроить фильтры для исключения ложных срабатываний
      - Проанализировать найденные директории и файлы

5. **Дополнительные проверки безопасности:**
      - Проверить наличие файлов конфигурации (.env, config.php, backup.sql)
      - Поиск файлов с расширениями .bak, .old, .backup
      - Проверка на наличие административных панелей (/admin, /wp-admin, /phpmyadmin)
      - Анализ HTTP заголовков на предмет утечки информации
      - Проверка на наличие открытых директорий без index файлов

6. **Документирование результатов:**
      - Сделать скриншоты всех попыток взлома
      - Описать каждую проверенную уязвимость
      - Указать успешность каждой попытки
      - Записать найденные интересные файлы или директории
      - Сформулировать выводы о безопасности сайта

**Важные ограничения:**
   - БЕЗ DDoS атак и подбора паролей
   - БЕЗ использования найденных уязвимостей в злонамеренных целях
   - При обнаружении реальной уязвимости связаться с преподавателем
   - Цель - изучение влияния неточностей в конфигурации на безопасность

**Результат:** Вы должны понимать основные уязвимости веб-приложений и уметь их тестировать этично.


### Полезные ссылки

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Node Exporter](https://github.com/prometheus/node_exporter)
- [Docker Compose Documentation](https://docs.docker.com/compose/)
- [Prometheus Python Client](https://github.com/prometheus/client_python)

