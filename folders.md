<!-- TITLE: Структура папок -->
<!-- SUBTITLE: Описание структуры папок -->

# Структура папок

Особенностью плагинов и модулей, написанных  под ScriptCraft является то, что для них отведена отдельная директори , лежащая в корневой дирректории вашего сервера - `/Путь к папке с вашим сервером./scriptcraft`

### Структура папок создаваемая плагином `ScriptCraft`:

```text
./scriptcraft   - папка плагина ScriptCraft находится в корневой дирректории сервера
  ─┬─────────
   ├ data       - папка предназначена для хранения данных плагинов и модулей
   ├ lib        - библиотеки функций, загружаются командой require()
   ├ modules    - подключаемые модули, загружаются командой require()
   └ plugins    - папка с плагинами, загружаются автоматически

```

Следует отметить, что при загрузке сервера, ScriptCraft производит рекурсивный поиск в папке `plugins` и ее подпапках, и загружает все найденные файлы с расширением `.js`.

В процессе написания своих модулей и плагинов, пришел к необходимости структурировать файлы данных. Для этих целей по структуре папки `./scriptcraft/data` было принято следующее решение (касаемое только моих плагинов и модулей):

```text
./scriptcraft   - папка плагина ScriptCraft находится в корневой дирректории сервера
  ─┬─────────
   ├ data       - папка предназначена для хранения данных плагинов и модулей
   │ ─┬──
   │  ├ config        - папка предназначена для хранения конфигурационных файлов плагинов и модулей
   │  │ ─┬────
   │  │  ├ modules    - папка предназначена для хранения конфигурационных файлов модулей
   │  │  │
   │  │  │
   │  │  └ plugins    - папка предназначена для хранения конфигурационных файлов плагинов
   │  │ 
   │  ├ data          - папка предназначена для хранения файлов данных плагинов и модулей
   │  │ ─┬────
   │  │  ├ modules    - папка предназначена для хранения файлов данных модулей
   │  │  │
   │  │  │
   │  │  └ plugins    - папка предназначена для хранения файлов данных плагинов
   │  │ 
   │  └ locales       - папка предназначена для хранения файлов локализации плагинов и модулей
   │    ─┬────
   │     ├ modules    - папка предназначена для хранения файлов локализации модулей
   │     │
   │     │
   │     └ plugins    - папка предназначена для хранения файлов локализации плагинов
   │
   │
   ├ lib        - библиотеки функций, загружаются командой require()
   ├ modules    - подключаемые модули, загружаются командой require()
   └ plugins    - папка с плагинами, загружаются автоматически

```

Так же, с целью обезопасится от конфликта имен файлов и папок, все мои модули и плагины, а также их файлы с настройками, локалями и данными хранятся в собственной подпапке `last` (от моего ника в minecraft `lastuniverse`)

В качестве примера, привожу полную структуру для моих модулей
```text
./scriptcraft
  ─┬─────────
   ├ data
   │ ─┬──
   │  ├ config
   │  │ ─┬────
   │  │  ├ modules
   │  │  │ ─┬─────
   │  │  │  └ last    - в папке находятся конфигурационные файлы моих модулей
	 │  │  │
   │  │  └ plugins
   │  │    ─┬─────
   │  │     └ last    - в папке находятся конфигурационные файлы моих плагинов
   │  │ 
   │  ├ data
   │  │ ─┬────
   │  │  ├ modules
   │  │  │ ─┬─────
   │  │  │  └ last    - в папке находятся файлы данных для моих модулей
	 │  │  │
   │  │  └ plugins
   │  │ 
   │  └ locales
   │    ─┬────
   │     ├ modules
   │     │ ─┬─────
   │     │  └ last    - в папке находятся папки (по имени модуля) с файлами локализации для моих модулей
	 │     │
   │     └ plugins    - папка предназначена для хранения файлов локализации плагинов
   │
   ├ lib        - библиотеки функций, загружаются командой require()
   ├ modules    - подключаемые модули, загружаются командой require()
   └ plugins    - папка с плагинами, загружаются автоматически

```