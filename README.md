# 2025_2026 — introduction-in-web-tech — U4255 — Yangalin I. A.

Учебный репозиторий по дисциплине **«Введение в веб-технологии»**, факультет инфокоммуникационных технологий (ФИКТ), Университет ИТМО.

- **Год:** 2025/2026
- **Студент:** Янгалин Ислам Азаматович
- **Группа:** U4255
- **Курс на GitHub:** <https://github.com/itmo-ict-faculty/introduction-in-web-tech>
- **Сайт курса:** <https://itmo-ict-faculty.github.io/introduction-in-web-tech/>

> Имя финального репозитория для сдачи: `2025_2026-introduction-in-web-tech-U4255-yangalin_i_a`.

## Содержимое

| Папка | Лабораторная |
|-------|--------------|
| [`lab0/`](lab0/) | №0 — Создание репозитория и настройка окружения |
| [`lab1/`](lab1/) | №1 — Основы Docker |
| [`lab2/`](lab2/) | №2 — CI/CD на GitHub Actions + Docker Hub |
| [`lab3/`](lab3/) | №3 — Мониторинг с Prometheus и Grafana |
| [`coursework/`](coursework/) | Курсовая — персональный сайт на MkDocs |
| [`docs/`](docs/) | Локальные копии официальных спецификаций курса |

## План обучения

1. Лекции (~30 ч) — список в [docs/lectures.md](docs/lectures.md).
2. Лабораторные работы (~24 ч) — обязательные для зачёта.
3. Курсовая — по желанию.

## Личный план изучения DevOps

Сквозная цель — освоить полный цикл от исходного кода до production-мониторинга. Опорные темы взяты из [списка лекций курса](docs/lectures.md):

1. **Git и code review** — ветвление (`main` / `develop` / `lab<N>/...`), pull requests, защищённые ветки. Закрепляется на lab0.
2. **Контейнеризация (Docker)** — образы, Dockerfile, multi-stage builds, реестры. Закрепляется на lab1.
3. **CI/CD (GitHub Actions)** — автоматическая сборка и пуш образов в Docker Hub, repository secrets. Закрепляется на lab2.
4. **Мониторинг и логирование (Prometheus + Grafana + Node Exporter)** — метрики, дашборды, алерты. Закрепляется на lab3.
5. **Сети и протоколы** — модель OSI/TCP-IP, HTTP, обратные прокси (nginx). Используется во всех лабах.
6. **Оркестрация (Kubernetes)** — Pods, Services, Deployments, стратегии масштабирования. Самостоятельное изучение после курса.
7. **Infrastructure as Code (Terraform / Ansible)** — декларативное описание инфраструктуры, версионирование. Самостоятельное изучение после курса.

## Контакты

- Email: <ara8ella@gmail.com>
- GitHub: [Iangalin](https://github.com/Iangalin)

## Лицензия

[MIT](LICENSE)
