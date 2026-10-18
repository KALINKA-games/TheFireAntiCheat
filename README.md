# TheFireAntiCheat v4.0 🛡️

Продвинутая система защиты для серверов Minecraft PaperMC 1.21.x с полной системой отчетности, веб-панелью и детекцией читов без использования ProtocolLib.

## ✨ Особенности

- **Без ProtocolLib**: Работает на чистом Netty инжекторе и Bukkit API
- **Java 21**: Полная поддержка современного JDK
- **Асинхронная БД**: Все операции с SQLite выполняются в отдельных потоках (нулевое влияние на TPS)
- **X-Ray Иллюзия**: Подмена всех руд на алмазную через `sendBlockChange` (без NMS)
- **Веб-панель**: Локальный дашборд на `localhost:8080` с авторизацией по никнейму
- **Многоязычность**: Русский и английский языки (настраивается в конфиге)
- **LuckPerms интеграция**: Гибкая система прав для команд и отчетов
- **13+ Детекторов**: Killaura, NoFall, AutoCrystal, Scaffold, BrandCheck и другие
- **Экспорт данных**: CSV/JSON экспорт истории нарушений
- **Безопасность**: AES-шифрование БД, автобэкапы, rate-limiting команд

---

## 📋 Требования

| Компонент | Версия | Примечание |
|-----------|--------|------------|
| **Сервер** | PaperMC 1.21.x | Folia также поддерживается |
| **Java** | JDK 21 | Temurin / Adoptium рекомендуется |
| **RAM** | +512 MB - 1 GB | Зависит от онлайна |
| **ProtocolLib** | ❌ Не нужен | Плагин работает нативно |

---

##  Установка

1. Скомпилируйте проект в IntelliJ IDEA: **Build → Build Project** (`Ctrl+F9`)
2. Найдите файл `target/TheFireAntiCheat.jar`
3. Скопируйте его в папку `plugins/` вашего сервера
4. Перезапустите сервер (`/restart`)
5. После первого запуска создастся папка `plugins/TheFireAntiCheat/` с конфигурацией

---

## ️ Настройка config.yml

После установки откройте `plugins/TheFireAntiCheat/config.yml`:

```yaml
language: ru                    # 'ru' или 'en'
db-version: 2                   # версия схемы БД (не менять вручную)

database:
  filename: "reports.db"        # имя файла базы данных
  encryption:
    enabled: false              # шифрование БД (требует ключ 32+ символов)
    key: "CHANGE_THIS_KEY_32_CHARS_MINIMUM!!"
  backup:
    enabled: true               # автоматические бэкапы
    interval-hours: 24          # интервал бэкапов
    max-backups: 5              # максимальное количество бэкапов

rate-limit:
  report-cooldown-seconds: 5    # кулдаун команды /report
  stats-cooldown-seconds: 3     # кулдаун команды /reportstats

ping-compensation:
  base-threshold: 700           # базовый порог пинга
  multiplier: 0.3               # адаптивная компенсация (threshold = base + ping * multiplier)

# Настройки детекторов
diamond-detection:
  threshold: 5                  # количество алмазов за период
  period-seconds: 60            # период времени в секундах
  max-violations: 3             # VL перед созданием репорта

autocrystal:
  min-place-interval: 150       # минимальный интервал установки кристаллов (мс)
  max-distance: 7.0             # максимальная дистанция установки
  report-threshold: 8           # порог VL

antiafk:
  idle-time: 120                # время бездействия (секунды)
  check-count: 5                # количество проверок подряд
  report-threshold: 3           # порог VL

scaffold:
  max-blocks-per-second: 12     # максимальная скорость строительства
  max-pitch-angle: 85.0         # максимальный угол взгляда вниз
  report-threshold: 10          # порог VL

inventory-move:
  clicks-per-second: 15         # максимальная скорость кликов в инвентаре
  report-threshold: 8           # порог VL

brand-check:
  enabled: true                 # проверка бренда клиента
  allowed-brands:               # список легитимных клиентов
    - "vanilla"
    - "fabric"
    - "forge"
    - "quilt"
    - "neoforge"
    - "lunar"
    - "badlion"
    - "feather"
    - "labymod"
    - "optifine"
  action: REPORT                # действие: REPORT или KICK

protocol-anomaly:
  enabled: true                 # детекция аномалий протокола
  min-look-interval-ms: 5       # минимальный интервал пакетов поворота
  max-duplicate-packets: 3      # максимальное количество дубликатов
  report-threshold: 5           # порог VL

web-panel:
  enabled: true                 # включить веб-панель
  port: 8080                    # порт для localhost
  security:
    auto-whitelist-ops: true    # автодобавление операторов в whitelist
    allowed-players: []         # ручные никнеймы для доступа
  generate-login-link: true     # показывать ссылку в консоли при старте

archive:
  enabled: true                 # архивация старых репортов
  days-to-archive: 90           # возраст репортов для архивации

metrics:
  enabled: true                 # сбор метрик производительности
  log-interval-minutes: 5       # интервал логирования метрик
