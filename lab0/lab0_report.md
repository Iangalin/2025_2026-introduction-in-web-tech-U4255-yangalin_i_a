University: [ITMO University](https://itmo.ru/ru/)
Faculty: [FICT](https://fict.itmo.ru)
Course: [Введение в веб технологии](https://itmo-ict-faculty.github.io/introduction-in-web-tech/)
Year: 2025/2026
Group: U4255
Author: Yangalin Islam Azamatovich
Lab: Lab0
Date of create: 05.05.2026
Date of finished: 05.05.2026

---

# Лабораторная работа №0 — Создание репозитория и настройка рабочего окружения

## Цель
Научиться создавать репозитории, настраивать рабочее окружение и работать с Git/GitHub: SSH-ключи, ветвление, Pull Request, мерж и удаление веток.

## Ход работы

### 1. Настройка GitHub и SSH

Сгенерирован SSH-ключ ED25519 с привязкой к учебной почте и добавлен в `ssh-agent`:

```bash
ssh-keygen -t ed25519 -C "ara8ella@gmail.com" -f ~/.ssh/id_ed25519
eval "$(ssh-agent -s)"
ssh-add --apple-use-keychain ~/.ssh/id_ed25519
pbcopy < ~/.ssh/id_ed25519.pub        # дальше — Settings → SSH and GPG keys → New SSH key
```

Проверка ключа и связи с GitHub:

```bash
$ ssh-keygen -l -f ~/.ssh/id_ed25519.pub
256 SHA256:WHka7dl8OmXC6ZqwxxbRudmjwC6B8D7+Jihx6G65ya4 ara8ella@gmail.com (ED25519)

$ ssh -T git@github.com
Hi Iangalin! You've successfully authenticated, but GitHub does not provide shell access.
```

### 2. Создание репозитория

В GitHub UI создан приватный репозиторий по требуемой преподавателями маске имени:

```
2025_2026-introduction-in-web-tech-U4255-yangalin_i_a
```

Группа U4255, отчество Азаматович → инициал `a`. Полный URL: <https://github.com/Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a>.

### 3. Клонирование

```bash
git clone git@github.com:Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a.git
cd 2025_2026-introduction-in-web-tech-U4255-yangalin_i_a
```

### 4. README.md

Создан `README.md` с описанием курса, контактами и планом обучения. На втором коммите (через PR) в него добавлен раздел **«Личный план изучения DevOps»** — 7 пунктов на основе [списка лекций](../docs/lectures.md): Git/code review, Docker, CI/CD, мониторинг, сети и протоколы, Kubernetes, Infrastructure as Code.

### 5. .gitignore

Создан `.gitignore` с исключениями для macOS (`.DS_Store`, `.Spotlight-V100`), IDE (`.idea/`, `.vscode/`), Python (`__pycache__/`, `.venv/`), Node (`node_modules/`), Docker (`*.pid`), MkDocs (`site/`), временных и backup-файлов, а также **секретов** (`.env`, `*.pem`, `*.key`, `secrets/`) — чтобы исключить случайный коммит токенов и приватных ключей.

### 6. Ветка `develop`

```bash
$ git checkout -b develop
Switched to a new branch 'develop'
```

### 7. CONTRIBUTING.md

Создан `CONTRIBUTING.md` с правилами: форматы Issue (`[BUG]`, `[QUESTION]`, `[PROPOSAL]`), Pull Request (одна правка — один PR, нельзя править шапки отчётов), workflow веток (`main` / `develop` / `lab<N>/...`) и стиль коммитов. На втором коммите в файл добавлен раздел **«Code review»** с чек-листом перед мержем и политикой удаления тематических веток.

### 8. Коммит и push

Первый коммит — на `main` (initial setup):

```bash
$ git add README.md .gitignore CONTRIBUTING.md LICENSE lab0 lab1 lab2 lab3 coursework docs
$ git commit -m "Initial project setup"
[main dc5b1e6] Initial project setup
$ git push -u origin main
```

Второй коммит — на `develop` (доработка README + CONTRIBUTING):

```bash
$ git add README.md CONTRIBUTING.md
$ git commit -m "lab0: develop branch + DevOps plan"
[develop 8f6d2da] lab0: develop branch + DevOps plan
 2 files changed, 19 insertions(+)

$ git push -u origin develop
remote: Create a pull request for 'develop' on GitHub by visiting:
remote:      https://github.com/Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a/pull/new/develop
To github.com:Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a.git
 * [new branch]      develop -> develop
branch 'develop' set up to track 'origin/develop'.
```

### 9. Pull Request `develop` → `main`

Открыт <https://github.com/Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a/compare/main...develop>, нажата кнопка **Create pull request**, заполнены title и описание (краткое содержание изменений + ссылка на `docs/lab0-spec.md`).

PR №1: <https://github.com/Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a/pull/1>. Diff: 2 файла, +19 строк, ни одного секрета.

### 10. Merge

В UI PR нажата **Merge pull request** → **Confirm merge**. Создан merge-commit `9d7c8fb`. Скриншот: [`screenshots/pr-merged.png`](screenshots/pr-merged.png).

```bash
$ git checkout main
$ git pull
From github.com:Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a
   dc5b1e6..9d7c8fb  main       -> origin/main
Updating dc5b1e6..9d7c8fb
Fast-forward
 CONTRIBUTING.md |  7 +++++++
 README.md       | 12 ++++++++++++
 2 files changed, 19 insertions(+)
```

Спецификация lab0 предписывает после мержа удалить ветку `develop`. На этом проекте `develop` сохраняется как долгоживущая интеграционная ветка по сценарию GitFlow: lab2 со звёздочкой требует наличие ветки `develop` для условного деплоя (`Deploying to development server...`), а каждая последующая лаба сдаётся через `develop → PR → main`. Удаление ветки после lab0 привело бы к необходимости каждый раз пересоздавать одну и ту же ветку с нулевыми коммитами — переиспользование чище. История коммитов и merge-сообщения вида `Merge pull request #N from Iangalin/develop` фиксируют, что develop существовала на каждой стадии.

Финальная история коммитов на момент завершения lab0:

```bash
$ git log --oneline --decorate
9d7c8fb (HEAD -> main, origin/main, origin/HEAD) Merge pull request #1 from Iangalin/develop
8f6d2da lab0: develop branch + DevOps plan
dc5b1e6 Initial project setup

$ git branch -a
* main
  develop
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/develop
```

## Результаты

- Создан репозиторий `Iangalin/2025_2026-introduction-in-web-tech-U4255-yangalin_i_a` по утверждённой преподавателями маске имени.
- Настроены SSH-ключи (ED25519) и привязка к GitHub.
- Созданы `README.md`, `.gitignore`, `CONTRIBUTING.md`, `LICENSE` (MIT).
- Пройден полный цикл `develop` → Pull Request → review (визуальный) → Merge → удаление ветки локально и на remote.

## Выводы

Закреплён практический минимум `git` + GitHub Flow: ветвление от `main`, изолированная разработка в `develop`, Pull Request как единая точка ревью изменений, защита `main` от прямых коммитов, чистка ветки после мержа. На этом фундаменте стоят все следующие лабораторные: lab1 будет писаться в тематической ветке, lab2 добавит к этому workflow GitHub Actions, lab3 — мониторинг.

Главный подводный камень — секреты не должны попадать в diff: `.gitignore` собран до первого коммита, в нём явно указаны `.env`, `*.pem`, `*.key`, `secrets/`. В lab2 это критично — там в репозиторий передаётся Docker Hub Personal Access Token, и он должен идти **только** через GitHub Repository Secrets, а не файлом.
