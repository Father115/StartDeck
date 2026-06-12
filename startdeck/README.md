# StartDeck

**StartDeck** — локальный лаунчер профилей на **React + Electron** для Windows.

Приложение запускает выбранный набор программ, папок, файлов, сайтов и команд одной кнопкой. Удобно для рабочих наборов, игр, учёбы, записи видео, диагностики ПК и любых повторяющихся сценариев.

## Возможности

- Профили запуска: например `Работа`, `Игры`, `Учёба`, `Запись`, `Диагностика`.
- Запуск всего профиля одной кнопкой.
- Запуск отдельного элемента профиля.
- Поддержка программ, папок, файлов, сайтов и команд.
- Выбор `.exe`, файла или папки через системное окно.
- Временное отключение элементов без удаления из профиля.
- Проверка существования путей.
- Журнал запуска и ошибок.
- Защита от повторного запуска процесса, если указано имя процесса.
- Пустой дефолтный конфиг без личных данных.
- Пользовательский конфиг хранится отдельно от проекта и может шифроваться через системные возможности Electron/Windows.

## Зачем это нужно

Обычно для начала работы приходится вручную открывать браузер, IDE, Discord/Telegram, папку проекта, VPN, OBS, Steam и другие программы. StartDeck позволяет собрать такие действия в профиль и запускать их одной кнопкой.

## Приватность

В репозитории и архиве по умолчанию **нет личных путей**.

Файл `data/apps.json` специально пустой:

```json
{
  "profiles": []
}
```

Безопасный пример лежит отдельно:

```text
data/example-apps.json
```

Он содержит только шаблонные значения вида:

```text
C:\Path\To\Program.exe
%USERPROFILE%\Path\To\Folder
https://example.com
```

После первого запуска реальные пользовательские профили сохраняются в папке данных приложения, а не в `data/apps.json` внутри проекта.

Обычно это будет что-то вроде:

```text
C:\Users\<user>\AppData\Roaming\StartDeck\profiles.encrypted.json
```

Если системное шифрование недоступно, приложение использует обычный файл:

```text
profiles.json
```

> Важно: не добавляй в GitHub свои реальные `profiles.json`, `profiles.encrypted.json`, `dist`, `node_modules` и другие локальные файлы. Для этого в проекте уже есть `.gitignore`.

## Установка для разработки

Нужны Node.js и npm.

```powershell
npm install
```

## Запуск в режиме разработки

```powershell
npm run dev
```

## Сборка portable `.exe`

```powershell
npm run dist:win
```

Готовый файл появится примерно здесь:

```text
dist\StartDeck 0.3.1.exe
```

## Как пользоваться

1. Запусти StartDeck.
2. Создай профиль, например `Работа`.
3. Нажми `+ Добавить`.
4. Выбери тип элемента: программа, папка, файл, сайт или команда.
5. Для программы/папки/файла нажми `Выбрать`, чтобы не писать путь вручную.
6. Укажи имя процесса, если хочешь включить защиту от повторного запуска.
7. Включи или отключи нужные элементы тумблером.
8. Нажми `Запустить всё`.

## Пример структуры профиля

```json
{
  "profiles": [
    {
      "id": "work",
      "name": "Работа",
      "description": "Основной рабочий набор",
      "items": [
        {
          "id": "browser",
          "name": "Браузер",
          "type": "app",
          "path": "C:\\Path\\To\\Browser.exe",
          "enabled": true,
          "skipIfRunning": true,
          "processName": "Browser.exe"
        },
        {
          "id": "project-folder",
          "name": "Папка проекта",
          "type": "folder",
          "path": "%USERPROFILE%\\Projects",
          "enabled": true
        },
        {
          "id": "github",
          "name": "GitHub",
          "type": "url",
          "url": "https://github.com",
          "enabled": true
        }
      ]
    }
  ]
}
```

## Команды npm

```powershell
npm run dev       # запуск разработки
npm run build     # сборка React/Vite
npm run start     # запуск Electron без dev-сервера
npm run dist:win  # сборка portable exe для Windows
```

## Как залить проект на GitHub

### 1. Создай репозиторий на GitHub

На GitHub создай новый репозиторий, например:

```text
startdeck
```

Лучше не ставить галочки `Add README`, `Add .gitignore`, `Add license`, если ты заливаешь уже готовую папку проекта. Они уже есть или будут добавлены локально.

### 2. Открой PowerShell в папке проекта

```powershell
cd "C:\Users\<user>\Desktop\startdeck"
```

### 3. Инициализируй Git

```powershell
git init
git branch -M main
```

### 4. Проверь, что лишнее не попадёт в репозиторий

```powershell
git status
```

В репозиторий должны попадать исходники, например:

```text
electron/
src/
data/apps.json
data/example-apps.json
build/icon.ico
package.json
vite.config.js
README.md
.gitignore
```

Не должны попадать:

```text
node_modules/
dist/
profiles.json
profiles.encrypted.json
.env
*.log
```

### 5. Сделай первый коммит

```powershell
git add .
git commit -m "Initial StartDeck release"
```

### 6. Привяжи удалённый репозиторий

Замени `USERNAME` на свой GitHub-логин:

```powershell
git remote add origin https://github.com/USERNAME/startdeck.git
```

### 7. Отправь проект на GitHub

```powershell
git push -u origin main
```

После этого проект появится на GitHub.

## Как обновлять проект на GitHub после изменений

После каждого изменения:

```powershell
git status
git add .
git commit -m "Update StartDeck"
git push
```

## Что делать, если GitHub просит логин/пароль

GitHub не принимает обычный пароль для Git-операций через HTTPS. Обычно удобнее использовать:

- вход через GitHub Desktop;
- вход через Git Credential Manager;
- Personal Access Token вместо пароля;
- SSH-ключ.

Для первого раза проще всего использовать GitHub Desktop или авторизацию Git Credential Manager, которая появляется автоматически при `git push`.

## Публиковать `dist` или нет

Обычно в репозиторий **не заливают** `dist` и готовые `.exe`, потому что это результаты сборки.

Правильнее так:

- исходный код хранится в репозитории;
- готовый `.exe` прикладывается в GitHub Releases;
- `dist/` остаётся локальным и игнорируется через `.gitignore`.

## Стек

- React
- Vite
- Electron
- electron-builder

## Статус

Проект находится на стадии локального MVP. Основная цель — удобный личный лаунчер профилей без хранения личных путей в репозитории.
