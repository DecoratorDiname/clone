Назначение:
- Аналогично Bash-версии: обновление локального репозитория A, копирование содержимого в B, исключая папку .git.

Файл: sync_repos.ps1
# Требования: PowerShell 5+ (Windows) или PowerShell 7+ (跨 платформа)
param(
  [string]$RepoA = "$env:USERPROFILE\repoA",
  [string]$RepoB = "$env:USERPROFILE\repoB",
  [string]$LogFile = "$env:USERPROFILE\sync_repos.log"
)

function Write-Log {
  param([string]$Message)
  $line = "$(Get-Date -Format 'yyyy-MM-dd HH:mm:ss') - $Message"
  Write-Output $line
  Add-Content -Path $LogFile -Value $line
}

# Проверка путей
foreach ($p in @($RepoA, $RepoB)) {
  if (-not (Test-Path $p)) {
    Write-Log "Директория не найдена: $p"
    exit 1
  }
}

# Обновление RepoA
if (Test-Path (Join-Path $RepoA ".git")) {
  Push-Location $RepoA
  if (-not (Test-Path "$RepoA\.git")) { Pop-Location; Write-Log "RepoA не является git-репозиторием"; exit 1 }
  Write-Log "Выполнение git pull в $RepoA"
  $pull = git pull 2>&1
  if ($LASTEXITCODE -ne 0) { Write-Log "git pull вернул ошибку: $pull"; Exit 1 }
  Pop-Location
} else {
  Write-Log "REPO_A не содержит .git: $RepoA"
  exit 1
}

# Копирование содержимого из RepoA в RepoB, исключая .git
# Используем robocopy для копирования с заменой
$src = (Join-Path $RepoA "")
$dst = (Join-Path $RepoB "")
$robocopyArgs = @(
  "`"$src`"",
  "`"$dst`"",
  "/MIR",
  "/XD",".git"
)

# robocopy командой игнорирует скрытые системные файлы, но мы явно исключаем директорию .git
# Робочий вариант: сначала копируем, потом удаляем лишнее
Write-Log "Начало копирования содержимого из $RepoA в $RepoB, исключая .git"
# В Windows путь с пробелами, используем двойные кавычки
$cmd = "robocopy `"$src`" `"$dst`" /MIR /XD `.git`"
$rc = & cmd /c $cmd
if ($LASTEXITCODE -lt 1) {
  Write-Log "Robocopy завершился с кодом $LASTEXITCODE"
} else {
  Write-Log "Robocopy завершился с кодом $LASTEXITCODE"
}
Write-Log "Синхронизация завершена."
exit 0

Чтобы запустить через терминал и видеть окно:
- Bash: просто запустите оболочку; для явного отображения можно запустить через терминал и вручную выполнить скрипт.
- PowerShell: запустите файл через PowerShell и включите выполнение скриптов (Set-ExecutionPolicy RemoteSigned -Scope CurrentUser).