<!-- TITLE: еconomy -->
<!-- SUBTITLE: Описание модуля управления экономикой -->

# `еconomy` - Модуль управления экономикой
## Возможности:
- централизованное, автоматизированное управление ценами на сервере
- сбор статистики по операциям купли и продажы для каждого вида товара
- вычисление среднесерверной цены на товары на основе статистики спроса и предложения
- выполнение транзакций игровой валюты между игроками и сервисами

## Файлы конфигурации

***data/config/modules/last/economy/match_table.json***
Содержит сопоставления названий блоков в различных нотациях. Так же служит для объединения нескольких типов блоков, одинаковых с точки зрения цены (смотри в примере записи для TERRACOTTA).

```javascript
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

***data/config/modules/last/economy/craft_table.json***
Содержит данные о составе всех блоков и предметов, построенные на основе анализа данных из [файлов с рецептами](https://minecraftmain.ru/2017/03/v-minecraft-dobavyat-rpg-sostavlyauschuyu/), появившихся в майнкрафте начиная с версии 1.12. Файл используется при расчете цен во всех магазинах при загрузке сервера, а также в любое время при установке магазинов.
```javascript
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
