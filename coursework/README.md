# Курсовая — персональный сайт на MkDocs

Полная спецификация: [../docs/coursework-spec.md](../docs/coursework-spec.md).

## Quick start

```bash
# 1. Установить инструменты (один раз)
pip install mkdocs mkdocs-material
mkdocs --version

# 2. Инициализировать сайт прямо в этой папке
mkdocs new .

# 3. Запустить локальный сервер
mkdocs serve     # http://127.0.0.1:8000

# 4. Собрать статику для деплоя
mkdocs build     # появится папка site/ (она в .gitignore)
```

## Минимальные требования к результату
- Тема `material`.
- Минимум 4 страницы: Главная, О себе, Проекты, Контакты.
- Настроенная навигация в `mkdocs.yml`.
- Использование разных Markdown-конструкций: заголовки, списки, ссылки, картинки, цитаты, таблицы.

## Шаблон `mkdocs.yml` (стартовая точка)

```yaml
site_name: Islam Yangalin — personal site
site_description: ITMO FICT, 2025/2026
site_author: Islam Yangalin
# site_url: https://<github-username>.github.io/<repo>/

theme:
  name: material
  palette:
    - scheme: default
      primary: teal
  features:
    - navigation.tabs
    - navigation.sections
    - navigation.top
    - search.highlight
    - search.share

nav:
  - Главная: index.md
  - О себе: about.md
  - Проекты: projects.md
  - Контакты: contacts.md

plugins:
  - search
```

## Деплой (опционально)
GitHub Pages через `mkdocs gh-deploy` либо отдельный workflow в `.github/workflows/`.
