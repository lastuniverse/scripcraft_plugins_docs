<!-- TITLE: economy -->
<!-- SUBTITLE: Описание плагина `economy` -->

# Плагин экономики.

## Возможности:
- ввод в игру игровой валюты
- проведение финансовых операций между игроками
- запрос баланса для себя и других игроков
- запрос финансовых рейтинговых таблиц


## Команды
- `/economy help` : вызов справки
- `/economy money` : узнать свой баланс
- `/economy money {playername}` : узнать баланс другого игрока
- `/economy pay {playername} {money}` : перечислить другому игроку определенную сумму. Минимальная сума : 1. Дробные суммы округляются до целых в сторону уменьшения
- `/economy give {playername} {money}` : подарить от имени сервера другому игроку определенную сумму. Минимальная сума : 1. Дробные суммы округляются до целых в сторону уменьшения
- `/money` : алиас для команды ***/economy money***
- `/money {playername}` : алиас для команды ***/economy money {playername} ***
- `/pay {playername} {money}` : алиас для команды ***/economy pay {playername} {money}***
- `/give {playername} {money}` : алиас для команды ***/economy give {playername} {money}***
- `/top {page}` : выводит в чат список из 10 богатейших игроков сервера. Не обязательный параметр  ***{page}***  указывает номер страницы списка для прсмотра.

## Файл конфигурации data/config/plugins/last/economy.json
```js
{
    "locale": "ru_ru",                // Язык по умолчанию `ru_ru`
    "enable": true,                   // Включить/выключить плагин `true`/`false`
}
```

## Настройки разрешений для плагина `economy` 

**Права доступа:** *могут быть выставленны персонально для разных групп и отдельных пользователей*
```js
...
"permissions": {
		"last_econimy.give":     false,     // разрешение на подарок игроку игровых денег от имени сервера
}
...
```

**Параметры:** *могут быть выставленны персонально для разных групп и отдельных пользователей*
```js
...
"options": {
   // плагин не имеет персональных и групповых настроек
}
...
```

## Файлы локализации (поддерживаются модулем [modules/last/localses](/modiles/locales) )

Файлы локализации плагина находятся в папке `scriptcraft/data/locales/plugins/last/last_economy`. Вы можете создавать свои собственные файлы с локализациями под разные языки, и менять локализованные сообщения в уже существующих файлах.

Пример файла локализации для русского языка `scriptcraft/data/locales/plugins/last/last_economy/ru_ru.json`
```json
{
    "msg": {
        "deny": "вам не разрешено использовать эту команду."
    },
    "help": [
        "/economy help : эта справка\n",
        "/economy money: узнать свой баланс\n",
        "/economy money {playername}: узнать баланс игрока с ником {playername} (только если игрок в онлайне)\n",
        "/economy pay {playername} {money} : : передать игроку {playername} Жакониев {money} (только если игрок в онлайне)\n",
        "/economy give {playername} {money} : подарить игроку {playername} Жакониев {money} (только если игрок в онлайне, доступна только главному админу)\n",
        "/economy top {10|15|20} : посмотреть 10, 15 или 20 богатеев на сервере\n",
        "/money : то же самое что и /economy money\n",
        "/money {playername}: то же самое что и /economy money {playername}\n",
        "/pay {playername} {money} : то же самое что и /economy pay {playername} {money}\n",
        "/top {10|15|20} : то же самое что и /economy top {10|15|20}\n"    
    ]
}
```

 ## Важно
Плагин является всего лишь интерфейсом к модулю экономики [economy](/modules/economy)

## Зависимости:
> - utils - стандартный модуль ScriptCraft
> - [modules/last/economy](/modules/economy)     - модуль управления экономикой и финансами игрока
> - [modules/last/completer](/modules/completer)   - модуль регистрации команд /jsp commandname как глобальных команд /commandname с возможностью автодополнения
> - [modules/last/permissions](/modules/permissions) - модуль управления правами доступа к функционалц прагинов для пользователей и групп пользователей
> - [modules/last/locales](/modules/locales)     - модуль локализации
