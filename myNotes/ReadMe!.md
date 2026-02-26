## Назначение:
- Переключается в корневую директорию пользователя (или другой корень, заданный переменной).
- Выполняет git pull в указанном локальном репозитории (репозиторий A).
- Копирует все содержимое репозитория A в локальный репозиторий B, исключая директорию .git и проверяя корректность путей.
- Заменяет существующие файлы и папки в B.

Файл: sync_repos.sh
#!/usr/bin/env bash
set -euo pipefail

# Настройки
```USER_HOME="${HOME:-/root}"```

REPO_A="$USER_HOME/repoA"  # локальный путь к первому репозиторию

REPO_B="$USER_HOME/repoB"  # локальный путь ко второму репозиторию

LOG_FILE="${USER_HOME}/sync_repos.log"

# Функции
log() {
  echo "$(date '+%Y-%m-%d %H:%M:%S') - $*"
  echo "$(date '+%Y-%m-%d %H:%M:%S') - $*" >> "$LOG_FILE"
}

die() {
  log "FATAL: $*"
  exit 1
}

# Валидация путей
for p in "$REPO_A" "$REPO_B"; do
  [ -d "$p" ] || die "Директория не найдена: $p"
  [ -d "$p/.git" ] || true  # позволяем репозитории без инициализации .git на момент проверки
done

# Проверка git-активности в REPO_A
if [ -d "$REPO_A/.git" ]; then
  pushd "$REPO_A" >/dev/null || die "Не удаётся перейти в $REPO_A"
  # Пропуск приватности: предполагаем, что ключи/кэш настроены
  if ! git rev-parse --is-inside-work-tree >/dev/null 2>&1; then
    popd >/dev/null
    die "REPO_A не является git-рабочей копией"
  fi
  log "Запуск git pull в $REPO_A"
  git pull || die "git pull в $REPO_A не удался"
  popd >/dev/null
else
  die "REPO_A не содержит .git: $REPO_A"
fi

# Копирование содержимого из REPO_A в REPO_B, исключая .git
log "Копирование содержимого из $REPO_A в $REPO_B (за исключением .git)..."
rsync -a --delete \
  --exclude='.git' \
  "$REPO_A/" "$REPO_B/" \
  || die "Ошибка копирования через rsync"

log "Синхронизация завершена успешно."
exit 0

Как использовать:
- Установить переменные REPO_A и REPO_B на реальные пути к локальным репозиториям.
- Сделать скрипт исполняемым: chmod +x sync_repos.sh
- Запуск через терминал: ./sync_repos.sh
- В случае необходимости можно запускать в автозагрузке через systemd/user или автозагрузку терминала.
