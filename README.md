# Training

Учебный Python-проект.

## Окружение

```bash
cd /home/vafly/training
python3 -m venv .venv
source .venv/bin/activate
pip install -r requ.txt
```

---

## Памятка по Git

Git — **системная** программа. Устанавливается через `apt`, работает везде и **не зависит** от виртуального окружения Python (`.venv`).

### Первоначальная настройка

Глобальные настройки хранятся в `~/.gitconfig`:

```bash
git config --global user.name "v.shilov"
git config --global user.email "personal@example.com"
```

Проверить:

```bash
git config --global --list
```

### Две почты: личная и корпоративная

В `~/.gitconfig` можно указать только **одну** пару name/email по умолчанию. Для разных проектов есть два подхода.

#### Способ 1 — локально в каждом репозитории (простой)

Глобально — личная почта, в корпоративном проекте переопределяете локально:

```bash
cd ~/work/corp-project
git config user.email "you@company.com"
git config user.name "V. Shilov"
```

Локальные настройки пишутся в `.git/config` **только этого проекта** и перекрывают глобальные.

Проверить, какая почта будет в коммите:

```bash
git config user.email
```

#### Способ 2 — автоматически по папке (удобно при многих проектах)

В `~/.gitconfig`:

```ini
[user]
    name = v.shilov
    email = personal@example.com

[includeIf "gitdir:~/work/"]
    path = ~/.gitconfig-work

[includeIf "gitdir:~/corp/"]
    path = ~/.gitconfig-corp
```

Файл `~/.gitconfig-work`:

```ini
[user]
    email = you@company.com
    name = V. Shilov
```

Все репозитории внутри `~/work/` автоматически получат рабочую почту.

> **Важно:** email в коммите — это метка автора. На push/pull он не влияет, но виден в истории на GitHub/GitLab.

---

### Базовый цикл работы

```bash
# посмотреть изменения
git status
git diff

# добавить файлы в индекс
git add main.py
git add .                  # все изменённые файлы

# зафиксировать
git commit -m "добавил main.py"

# отправить на сервер
git push

# забрать изменения с сервера
git pull
```

### Клонирование и remote

```bash
# скачать чужой/свой репозиторий
git clone git@github.com:user/repo.git
cd repo

# посмотреть подключённые remote
git remote -v

# добавить remote
git remote add origin git@github.com:user/repo.git
```

### Ветки

```bash
git branch              # список веток
git branch feature-x    # создать ветку
git checkout feature-x  # переключиться
git switch feature-x    # то же (новый синтаксис)

git checkout -b feature-x   # создать и сразу переключиться
git merge feature-x         # влить в текущую ветку
```

### HEAD — где вы сейчас в истории

**HEAD** — указатель на текущий коммит, «голова» рабочей копии. Обычно HEAD смотрит на последний коммит активной ветки.

```bash
git log -1              # последний коммит — туда указывает HEAD
cat .git/HEAD           # ref: refs/heads/main — HEAD на ветке main
```

| Состояние | Что означает |
|---|---|
| HEAD → ветка → коммит | нормальный режим, вы на ветке `main` |
| detached HEAD | HEAD указывает прямо на коммит, не на ветку — новые коммиты легко «потерять» |

Переместить HEAD (осторожно):

```bash
git checkout main       # HEAD → main → последний коммит ветки
git switch -c feature   # создать ветку и перенести HEAD на неё
```

> Коммит — это снимок проекта. HEAD — «всему голова»: показывает, с какой версии вы работаете прямо сейчас.

### История и отмена

```bash
git log                 # история коммитов
git log --oneline       # кратко

git restore main.py     # отменить изменения в файле (до add)
git restore --staged main.py  # убрать из индекса (после add)

git reset HEAD~1        # отменить последний commit, файлы останутся
```

### .gitignore

Файл `.gitignore` в корне проекта — список того, что Git **не отслеживает**:

```
.venv
.vscode
__pycache__/
*.pyc
.env
```

Git игнорирует эти файлы во всех командах (`status`, `add`, `commit`).

---

### Частые команды — шпаргалка

| Команда | Что делает |
|---|---|
| `git status` | что изменилось |
| `git diff` | показать diff |
| `git add .` | подготовить все изменения к commit |
| `git commit -m "..."` | зафиксировать |
| `git push` | отправить на remote |
| `git pull` | скачать и слить с remote |
| `git clone URL` | скопировать репозиторий |
| `git branch` | список веток |
| `git log --oneline` | история |
| `git config user.email` | какая почта в этом repo |

---

### Git и Python venv — не путать

| | Git | pip / pytest |
|---|---|---|
| Установка | `sudo apt install git` | `pip install pytest` |
| Где живёт | `/usr/bin/git` (система) | `.venv/` (проект) |
| Зависит от `source .venv/bin/activate` | **Нет** | **Да** |

Prompt `(.venv)` в терминале не ограничивает git — он доступен всегда.
