<!-- TITLE: еconomy -->
<!-- SUBTITLE: Описание модуля управления экономикой -->

# `еconomy` - Модуль управления экономикой
## Возможности:
- централизованное, автоматизированное управление ценами на сервере
- сбор статистики по операциям купли и продажы для каждого вида товара
- вычисление среднесерверной цены на товары на основе статистики спроса и предложения
- выполнение транзакций игровой валюты между игроками и сервисами

## Файлы конфигурации
Для работы этого модуля используется несколько, достаточно объемных файлов с данными. 

### data/config/modules/last/economy.json
Содержит основные настройки модуля
```javascript
{
    "locale": "ru_ru",   // локаль по умолчанию
    "enable": true,      // при false модуль будет отключен
    "basecost": 1000     // опорная цена, ее изменение приводит к изменению всех цен на сервере
}
```


### data/config/modules/last/economy/match_table.json
Содержит сопоставления названий блоков в различных нотациях. Так же служит для объединения нескольких типов блоков, одинаковых с точки зрения цены (смотри в примере записи для TERRACOTTA).

```json
{
...
  "STONE": {
    "unit_name": "STONE",
    "minecraft_name": "minecraft:stone"
  },
  "GRASS": {
    "unit_name": "GRASS",
    "minecraft_name": "minecraft:grass"
  },
	...
  "WHITE_GLAZED_TERRACOTTA": {
    "unit_name": "GLAZED_TERRACOTTA",
    "minecraft_name": "minecraft:glazed_terracotta"
  },
  "ORANGE_GLAZED_TERRACOTTA": {
    "unit_name": "GLAZED_TERRACOTTA",
    "minecraft_name": "minecraft:glazed_terracotta"
  },
	...
}
```

### data/config/modules/last/economy/craft_table.json
Содержит данные о составе всех блоков и предметов, построенные на основе анализа данных из [файлов с рецептами](https://minecraftmain.ru/2017/03/v-minecraft-dobavyat-rpg-sostavlyauschuyu/), появившихся в майнкрафте начиная с версии 1.12. Файл используется при расчете цен во всех магазинах при загрузке сервера, а также в любое время при установке магазинов.
```json
{
...
  "minecraft:wooden_stairs": {
    "minecraft:log": 0.375
  },
  "minecraft:wooden_slab": {
    "minecraft:log": 0.125
  },
  "minecraft:activator_rail": {
    "minecraft:log": 0.0625,
    "minecraft:redstone": 0.16666666666666666,
    "minecraft:iron_nugget": 9
  },
...
  "minecraft:rabbit_stew": {
    "minecraft:baked_potato": 1,
    "minecraft:cooked_rabbit": 1,
    "minecraft:log": 0.1875,
    "minecraft:carrot": 1,
    "minecraft:red_mushroom": 1
  },
...
}
```

### data/config/modules/last/economy/base_table.json
Содержит базовые цены для базовых блоков и предметов (тех, из которых крафтятся все остальное, но сами они не крафтятся). Изменения цен в этом файле приведет к автоматическому изменению всех цен на все блоки и предметы во всех магазинах на сервере.
```json
{
...
  "minecraft:log": 100,
  "minecraft:redstone": 250,
  "minecraft:iron_nugget": 48,
  "minecraft:quartz": 100,
  "minecraft:cobblestone": 30,
...
  // )))))))))
  "minecraft:command_block": 99999999999999999,
  "minecraft:command_block_minecart": 99999999999999999,
  "minecraft:repeating_command_block": 99999999999999999,
  "minecraft:chain_command_block": 99999999999999999
...
}
```

### data/config/modules/last/economy/enchants_table.json
Содержит цены для всех чар 1-го уровня. Используется для подсчета цен книг заклинания и чареных предметов. Изменения цен в этом файле приведет к автоматическому изменению цен на все книги хаклинания и зачаренные предметы во всех магазинах на сервере.
```json
{
    "PROTECTION_ENVIRONMENTAL": {"id":0,    "name": "Защита",              "cost":100},
    "PROTECTION_FIRE":          {"id":1,    "name": "Защита от огня",      "cost":100},
    "PROTECTION_FALL":          {"id":2,    "name": "Мягкое приземление",  "cost":100},
    "PROTECTION_EXPLOSIONS":    {"id":3,    "name": "Защита от взрыва",    "cost":100},
    "PROTECTION_PROJECTILE":    {"id":4,    "name": "Защита от снаряда",   "cost":100},
    "OXYGEN":                   {"id":5,    "name": "Дыхание",             "cost":200},
    "WATER_WORKER":             {"id":6,    "name": "Родство с водой",     "cost":15000},
    "THORNS":                   {"id":7,    "name": "Шипы",                "cost":200},
    "DURABILITY":               {"id":34,   "name": "Прочность",           "cost":200},
    "DAMAGE_ALL":               {"id":16,   "name": "Острота",             "cost":50},
    "DAMAGE_UNDEAD":            {"id":17,   "name": "Урон нежити",         "cost":50},
    "DAMAGE_ARTHROPODS":        {"id":18,   "name": "Бич членистоногих",   "cost":50},
    "KNOCKBACK":                {"id":19,   "name": "Отбрасывание",        "cost":400},
    "FIRE_ASPECT":              {"id":20,   "name": "Поджог",              "cost":400},
    "LOOT_BONUS_MOBS":          {"id":21,   "name": "Мародерство",         "cost":200},
    "DIG_SPEED":                {"id":32,   "name": "Эффективность",       "cost":50},
    "SILK_TOUCH":               {"id":33,   "name": "Шелковое касание",    "cost":25000},
    "LOOT_BONUS_BLOCKS":        {"id":35,   "name": "Удача",               "cost":200},
    "ARROW_DAMAGE":             {"id":48,   "name": "Сила",                "cost":50},
    "ARROW_KNOCKBACK":          {"id":49,   "name": "Откидывание",         "cost":400},
    "ARROW_FIRE":               {"id":50,   "name": "Воспламенение",       "cost":15000},
    "ARROW_INFINITE":           {"id":51,   "name": "Бесконечность",       "cost":25000},
    "LURE":                     {"id":61,   "name": "Прикормка",           "cost":200},
    "LUCK":                     {"id":62,   "name": "Морская удача",       "cost":200},
    "MENDING":                  {"id":70,   "name": "Починка",             "cost":50000},
    "VANISHING_CURSE":          {"id":71,   "name": "Проклятье утраты",    "cost":1000}
}
```

## Файлы локализации (поддерживаются модулем [modules/last/localses](/modiles/locales) )

Файлы локализации плагина находятся в папке `scriptcraft/data/locales/modules/last/economy`. Вы можете создавать свои собственные файлы с локализациями под разные языки, и менять локализованные сообщения в уже существующих файлах.

Пример файла локализации для русского языка `scriptcraft/data/locales/modules/last/economy/ru_ru.json`
```json
{
    "msg": {
        "test": "1234567890",
        "deny": "вам не разрешено использовать эту команду.",
        "top":    "Топ",
        "give": {
            "player": "Порывшись по своим карманам, вы нашли ${coins} случано завалявшихся ${coins_name}.",
            "trade": "Вы успешно подарили игроку ${coins} ${coins_name}.",
            "all": "Вы успешно подарили всем игрокам ${coins} ${coins_name}."
        },
        "pay": {
            "player": "${player} перевел вам ${coins} ${coins_name}.",
            "trade": "Вы успешно перевели игроку ${player} ${coins} ${coins_name}."
        },
        "player_undefined": "Игрокa с ником ${player} не существует!",
        "not_coins": "У вас не хватает ${coins_name} для совершения операции!",
        "not_client": "К сожалению вы еще на стали клиентом нашего банка и не можете осуществлять переводы!",
        "player_not_coins": "У ${player} нет ${coins_name}.",
        "me_coins": "У ${player} сейчас ${coins} ${coins_name}!",

    },
    "coins": ["коин","коина","коинов"]
}
```

## Экспортируемые функции
`coinsDecline(player,amount)` - преобразовывет число монет в строку с указанием названия монет в правильном падеже и в локализации игрока
 * @param  {object} player объект игрока, для которого производится преобразование
 * @param  {number} amount количество монет
 * @return {string}        строка содержащая количество монет и их название в правильном падеже и в локализации игрока
 
`toInt(number)` - преобразовывает строку или число к целому числу
 * @param  {number|string} number число или строка содержащая число
 * @return {number}        целое число



