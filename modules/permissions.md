<!-- TITLE: permissions -->
<!-- SUBTITLE: описание модуля permissions -->

# `permissions` - модуль для создания различных разрешений и настроек для пользователей и групп, и контроля их исполнения

а налог стандартного `permissionEx` для плагинов  написанных под `ScriptCraft` и еще немно плюшек

Данный модуль содержит в себе основные функции для проверки предустановленных в файлах концигурации разрещений и получения заданных тамже параметров для плагинов.
По сути не в полной мере дублирует функционал плагина PermissionEx. Разработана из-за невозможности получить доступ к данным PermissionEx.
В целях ускорения работы при первом вызове, модуль загружает данные из файлов конфигурации и разворативает их в удобный для машинной обработки формат.

## Файлы конфигурации:

- minecraft_server_folder/scriptcraft/data/config/modules/last/permissions/groups.json - настройки групп пользователей.
- minecraft_server_folder/scriptcraft/data/config/modules/last/permissions/users.json - индивидуальные настройки пользователей.

## секции и параметры фала конфигурации групп:

- isEnable [true|false] - если установленно в false, модуль игнорирует эту группу как будто ее нет в файле коныигурации.
- priotity [number] - выжный параметр указывающий приоритет группы. Если пользователь является членом двух или более групп и все они или некоторые из них содержат одинаковые названия разрешений и/или параметров но с разными значениями установленными для них, модуль будет использовать значения из группы с наивысшим приоритетом. Так же следует отметить, что наивысшим приоритетом обладают разрешения и параметры указанные в персональных настройках пользователя.
- default - [true|false] - если установленно в true, то разрешения и параметры этой группы будут использоватся по умолчанию если запрашиваемое значение или параметр не описан ни в одной из групп, членом которых является пользователь. Такое поведение гарантируется даже если параметр isEnable для дефолтной группы выставлен в false.
- permissions - секция содержит разрешения для различных плагинов. смотрите примеры использования.
- options - секция содержит параметры для различных плагинов. смотрите примеры использования.

**Пример:**
```javascript
...
"guest": {                          // описание группы "guest"
    "isEnable": true,                   // группа включена
    "default": true,                    // группа по умолчанию
    "priotity": 10,                     // приоритет группы
    "permissions": {                    // разрешения группы
        "last_warp.warp": false         
    },
    "options": {                        // параметры группы
        "last_warp.max":0,
        "last_teleport.cost": 300
    }
},
"player": {                         // описание группы "player"
    "isEnable": true,                   // группа включена
    "priotity": 20,                     // приоритет группы
    "permissions": {                    // разрешения группы
        "last_elitra.slap": false,      // разрешение отключено, как если бы его вообще небыло
        //"last_spawn.spawn": true,     // разрешение отключено, как если бы его вообще небыло
        "last_spawn.sign.use": true,
        "last_spawn.sign.place": true,
        "last_warp.sign.use": true,
        "last_warp.sign.place": true,
        "last_warp.access": true,
    },
    "options": {                        // параметры группы
        "last_warp.max":0,
        "last_teleport.cost": 300
    }
},
...
```

## секции и параметры фала конфигурации пользователей:
- name - содержит никнэйм игрока
- groups - секция содержит названия групп членом которых является пользователь
- permissions - секция содержит разрешения для различных плагинов. смотрите примеры использования
- options - секция содержит параметры для различных плагинов. смотрите примеры использования

**Пример:**
```javascript
...
"2f8a6ef2-e971-4b9a-90b9-f54db65dc4b7":{
	"name": "Serrgy",
	"groups": {
		"player": true,
		"vip": true
	},
	"permissions":{
			// тут можно задать персональные разрешения для игрока
	},
	"options": {
		// тут можно задать персональные настройки для игрока
	}
},
...
```

## API:

***`getUserPermissions(user)` -  возвращает объект класса `UserPermission`, содержащий разрешения и параметры пользователя (групповые и индивидуальные), а также функции их проверки и получения***
 * @param  {object} `user` объект, содержащий данные пользователя
 * @return {object}      возвращает объект класса `UserPermission`, содержащий разрешения и параметры пользователя (групповые и индивидуальные), а также функции их проверки и получения
 
***`isPermission(permissionName)` - метод класса `UserPermission`. Возвращает `true` если для указанного пользователя установленно разрешение с именем `permissionName`, иначе возвращает `false`***
 * @param {string}         `permissionName` строка, содержащая название проверяемого разрешения. Например: `last_warp.sign.use`
 * @return {boolean}      возвращает `true` если для указанного пользователя установленно разрешение с именем `permissionName`, иначе возвращает `false`

***`getParam(paramName)` - метод класса `UserPermission`. Возвращает актуальное для пользователя, значение параметра `paramName`***
 * @param {string}         `paramName` строка, содержащая название получаемого параметра. Например: `last_warp.max`
 * @return {boolean}      возвращает актуальное для пользователя, значение параметра `paramName`
 
 ***`isGroup(groupName)` - метод класса `UserPermission`. Возвращает `true` если указанный пользователь является членом группы `groupName`, иначе возвращает `false`***
 * @param {string}         `groupName` строка, содержащая название проверяемой группы
 * @return {boolean}      возвращает `true` если указанный пользователь является членом группы `groupName`, иначе возвращает `false`
 
 ## Пример использования:
В этом примере будет создана простая команда /mobspawn {mobType},  которая будет выполнятся только в том случае, если у пользователя  прпописанно разрешение на ее выполнение `mobspawn.use`. при этом, если в настройках параметр `mobspawn.types` будет сожержать массив с разрешенными к спавну типами мобов. Для работы данного примера, для пользователя должны быть прописанны соответствующие групповые и/или персональные настройки в файлах конфигурации групп и/или пользователей модуля permissions.

***пример файла групповых настроек `minecraft_server_folder/scriptcraft/data/config/modules/last/permissions/groups.json`***
```javascript
{
    "guests": {             // группа гостей, им будет недоступна команда /mobspawn
        "isEnable": true,         // группа включена
        "default": true,          // группа по умолчанию
        "priotity": 10,           // приоритет группы
        "permissions": {          // разрешения группы
        },
        "options": {              // параметры группы
        }
    },
    "players": {            // описание группы игроков, для спавна им будут доступны только свиньи и кролики
        "isEnable": true,         // группа включена
        "default": false,
        "priotity": 20,           // приоритет группы
        "permissions": {          // разрешения группы
            "mobspawn.use": true         
        },
        "options": {              // параметры группы
            "mobspawn.types": {"PIG": true, "RABBIT": true }
        }
    },
    "administrators": {     // описание группы администраторов, для спавна им будут доступны свиньи и кролики и зомби
        "isEnable": true,         // группа включена
        "default": false,
        "priotity": 999,          // приоритет группы
        "permissions": {          // разрешения группы
            "mobspawn.use": true
        },
        "options": {              // параметры группы
            "mobspawn.types": {"PIG": true, "RABBIT": true, "ZOMBIE": true }
        }
    }
}
```

***пример файла персональных настроек `minecraft_server_folder/scriptcraft/data/config/modules/last/permissions/users.json`***
```javascript
{
...
    "2f8a6ef2-e971-4b9a-90b9-f54db65dc4b7":{
				// этот игрок "masha" не будет иметь доступа к команде /mobspawn
        "name": "masha",
        "groups": {
        },
        "permissions":{
        },
        "options": {
        }
    },
    "34a8b6f2-6e64-b9a2-0ad7-c58ac5db6347":{
				// этот игрок "petya" будет иметь доступ к команде /mobspawn с правами группы players
        "name": "petya",
        "groups": {
            "players": true
        },
        "permissions":{
        },
        "options": {
        }
    },
    "58ac5db-24e6-b90a-d7a2-c634734a8b6f2":{
				// этот игрок "vasya" будет иметь доступ к команде /mobspawn с правами группы administrators
        "name": "vasya",
        "groups": {
            "administrators": true
        },
        "permissions":{
        },
        "options": {
        }
    },		
    "cb6f25db-90a6-b24e-c638-d7a258a4734a":{
				// этот игрок "kolya" будет иметь доступ к команде /mobspawn с индивидуальными настройками (сможет спавнить только летучих мышей)
        "name": "kolya",
        "groups": {
            "guests": true
        },
        "permissions":{
				    "mobspawn.use": true
        },
        "options": {
				    "mobspawn.types": {"BAT": true }
        }
    },		
...
}
```

***ну и непосредственно сам плагин, реализующий команду***
```javascript
// подключаем модуль разрешений
var permissions = require('last/permissions');

// подключаем регистрации глобальных команд
var completer = require('last/completer');

// Регистрируем команду `/mobspawn {mobType}` (без обработчика), 
// но с функцией, которая при вызове, будет возвращать список мобов, разрешенных для пользователя 
completer.addPlayerCommand('hello',undefined,get_mob_list)
         .addComplete('@user',cmd_hello);

// создаем функцию-обработчик команды `/mobspawn {mobType}`
// в params[0] будет `{mobType}` введенное после /mobspawn
function cmd_hello(params , sender){
    
		// получаем объект с опциями и разрешениями для пользователя который ввел команду
		var permission = permissions.getUserPermissions(sender);
    
		// проверяем у него наличие разрешения mobspawn.use
		// игрок "masha" отвалится именно тут
		if ( !permission.isPermission("mobspawn.use") )
        return echo(sender, "У вас нет разрешения на использования команды /mobspawn {mobType}");
		
		// получаем введенный игроком тип моба
		var mobType = params[0];
		
		// получаем список рпазрешенных к спавну мобов
		// игрок "petya" получит: {"PIG": true, "RABBIT": true }
		// игрок "vasya" получит: {"PIG": true, "RABBIT": true, "ZOMBIE": true }
		// игрок "kolya" получит: {"BAT": true }
		var mobList = permission.getParam("mobspawn.types");
		
		// проверяем имеет ли игрок право спавнить этот тип мобов
		if ( !mobList[mobType] )
        return echo(sender, "У вас нет разрешения спавнить "+mobType);
				
		// Все проверки пройдены, далее код, спавнящий указанного моба
		...
		
}


// создаем функцию, получающую разрешенный для пользователя список мобов
function get_mob_list( sender ) {

		// получаем объект с опциями и разрешениями для пользователя который ввел команду
		var permission = permissions.getUserPermissions(sender);

		// получаем список рпазрешенных к спавну мобов
		// игрок "petya" получит: {"PIG": true, "RABBIT": true }
		// игрок "vasya" получит: {"PIG": true, "RABBIT": true, "ZOMBIE": true }
		// игрок "kolya" получит: {"BAT": true }
		var mobList = permission.getParam("mobspawn.types");

		return (typeof mobList === "object"?mobList:{});
}
```

## Зависимости:
 
 - utils - стандартный модуль ScriptCraft
