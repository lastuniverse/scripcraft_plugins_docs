<!-- TITLE: locales -->
<!-- SUBTITLE:модуль поддержки локализации плагинов -->

# `locales` - модуль поддержки локализации плагинов
## Interface for working with locales
This module contains the basic functions of downloading localizations and sending localized messages to users

## configuration file for the module locales.js: `data/config/modules/locales.json`

```javascript
{
    "enable": true,
    "default": "ru_ru",       // default locale.
    "colors":{
        "event": "brightgreen", // the color of messages sent by the event function
        "echo":  "darkgreen",   // the color of messages sent by the warn function
        "warn":  "red",         // the color of messages sent by the warn function
        "help":  "aqua"         // the color of messages sent by the help function
    }
}
```

## file with messages in English: `data/locales/plugin/test/en_us.json`
```json
{
    "msg":{
        "test1": "test1 message",
        "test2": "test3 message",
    },
    "help": [
        "help1 message",
        "help2 message"
    ],
    "test": "test message ${key1} ${key2}"
}
```

## file with messages in Russian: `data/locales/plugins/test/ru_ru.json`
```json
{
    "msg":{
        "test1": "тест1 сообщение",
        "test2": "тест2 сообщение",
    },
    "help": [
        "хелп1 сообщение",
        "хелп2 сообщение"
    ],
    "test": "тест сообщение ${key1} ${key2}"
}
```

## example of a plugin using locales.js: plugins/test.js
```javascript
// connect the module
var  locales = require('last/locales');
// load the locale. The first parameter is the path, the second is the module name, the third is the default language of the plug-in
var locale = locales.init("./scriptcraft/data/locales/plugins/", "test", "ru_ru");
...

// !!! suppose that the default locale of the plugin is "ru_ru". And the user in his minecraft client exposed English
locale.help(player,"${help}"); 
// output to chat:
//   <playername> help1 message
//   <playername> help2 message
locale.echo(player,"${msg.test1}"); 
// output to chat:
//   <playername> test1 message
locale.echo(player,"${msg.test2}"); 
// output to chat:
//   <playername> test2 message
locale.echo(player,"${test}",{"key1": 11111, "key2": "abcdef" }); 
// output to chat:
//   <playername> test message 11111 abcdef

locale.warn(player,"${help.0}"); 
// output to chat:
//   <playername> help1 message
locale.warn(player,"aaa ${help.0} bbb ${msg.test1} ccc"); 
// output to chat:
//   <playername> aaa help1 message bbb test1 message ccc
locale.warn(player,"aaa ${help.0} bbb ${msg.test1} ccc ${test} ddd",{"key1": 11111, "key2": "value of key2" }); 
// output to chat:
//   <playername> aaa help1 message bbb test1 message ccc test message 11111 value of key2 ddd
// if there is no localization file for the player's language, the messages will be displayed in the language specified when calling locales.init(...)
// locale.warn(...), locale.help(...), locale.echo(...) and locale.event(...) differ only in text messages, otherwise their functionality is identical.
// more details on the capabilities of the module, you can read the description of its functions.
```