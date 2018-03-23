<!-- TITLE: Completer -->
<!-- SUBTITLE: модуль позволяет регистрировать глобальные автодополняемые команды  -->

# `complater` - API для регистрации глобальных автодополняемых команд
Данный модуль содержит в себе основные функции регистрации команд для плагинов ScriptCraft-a.
Так как по умолчанию все команды для плагинов написанных в ScriptCraft-е необходимо начинать с /jsp
что не совсем удобно, была предпринята попытка написать общесистемный обработчик, позволяющий регистрировать
команды для использования в глобальном пространстве имен команд, автодополнения их по нажатию TAB и привязки функций
обработчиков к каждой конкретной команде и/или ее дополнительному параметру.

## Модуль устанавливает обработчики событий для:
- [org.bukkit.event.server.TabCompleteEvent]{@link https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/event/server/TabCompleteEvent.html} - обработка и модификация автодополнений для сообщений вводимых как в чате клиента так и в консоли сервера.
- [org.bukkit.event.player.PlayerChatTabCompleteEvent]{@link https://hub.spigotmc.org/javadocs/spigot/org/bukkit/event/player/PlayerChatTabCompleteEvent.html} - к сожалению в ScriptCraft этот обработчик не вызывается хотя и должен, его функции в данном модуле возложены на org.bukkit.event.server.TabCompleteEvent
- [org.bukkit.event.player.PlayerCommandPreprocessEvent]{@link https://hub.spigotmc.org/javadocs/bukkit/org/bukkit/event/player/PlayerCommandPreprocessEvent.html} - перехват запросов на исполнение команд введенных в чате пользователя и переадрисация их на обработчики зарегестрированные данным модулем
- [org.bukkit.event.server.ServerCommandEvent]{@link https://jd.bukkit.org/org/bukkit/event/server/ServerCommandEvent.html} - перехват запросов на исполнение команд введенных в консоли сервера и переадрисация их на обработчики зарегестрированные данным модулем

## Возможные варианты имен для регистрации:
- строка содержащая набор символов без пробелов, дополнятся будут если эта строка начинается с тех же символов, что уже ввел пользователь при вводе команды.
- "@user" - дополнятся будет из списка игроков находящихся в онлайне.
- "@any" - дополнятся будет всегда, не зависимо от того что ввел игрок.
- "@re/.../" - дополнятся будет если ввод игрока совпал с указанным регулярным выражением. Например если надо отследить ввод игроком только цифр то следутет поставить "@re/[0-9]+/"

## Зависимости:
> - utils - стандартный модуль ScriptCraft
> - [modules/last/locales](/modules/locales)     - модуль локализации
> - [modules/last/permissions](/modules/permissions) - модуль управления правами доступа к функционалц прагинов для пользователей и групп пользователей

## API
`addGlobalCommand(comandName, handler, completer, permission)` - производит регистрацию псевдонима команды как глобальной (команда будет работать как из консоли сервера так и из чата игры).
 * @param   {string}    `comandName`  название псевдонима. например "/warp"
 * @param   {function}  `handler`     функция-обработчик для данного участка команды (необязательный параметр)
 * @param   {function}  `completer`   функция формирующая дополнительный список автодополнений для следующего участка команды (необязательный параметр)
 * @param   {string}    `permission`  название разрешения, проверятся при автодополнении команды (необязательный параметр)
 * @return  {object}    Экземпляр класа Completer содержащий методы для добавления дополнительных параметров к команде и их обработчиков и функций автодополнения




## Примеры использования:

**простой пример**
```javascript
//   подключаем модуль регистрации и автодополнения команд
var  completer = require('last/completer');
//   регистрируем команду {/youcomand} и ee обрабочтик как команду для чата клиента
var  command = completer.addPlayerCommand('youcomand',function (...){...});
//   регистрируем команду {/youcomand help} и ее обрабочтик как команду для чата клиента
     command.addComplete('help',function (...){...});
//   регистрируем команду {/youcomand data} без обработчика как команду для чата клиента
var  command_data = command.addComplete('data');
//   регистрируем команду {/youcomand data get <username> } и ее обрабочтик как команду для чата клиента
     command_data.addComplete('get').addComplete('@user',function (...){...});
//   регистрируем команду {/youcomand data set <username> <number> } и ее обрабочтик как команду для чата клиента
     command_data.addComplete('get').addComplete('@user').addComplete('@re/\d+/',function (...){...});
// теперь команда /youcomand со всеми ее параметрами будет доступна как глобальная, и будет автодополнятся по нажатию TAB
```
**пример демонстрирующий весь спектр возможностей**
```javascript
// В данном примере будет рассммотрено как зарегестрировать команды:
// /description help
// /description set {username} {you description}
// /description set {username} {email}
// /description delete {username}
// /description list 

// где:
// {username} - имя онлайн или офлайн игрока (онлайн игроки автодополняются по TAB)
// {you description} - произвольный текст

// Подключаем модуль регистрации и автодополнения команд
var  completer = require('last/completer');

// Создаем/загружаем хранилище данных для description
var store = persist('description', {} );

// Создаем массив с командами и их описаниями
var help_messages = [
"/description help - эта справка\n",
"/description set {username} {you description} - запомнить для игрока {username} описание {you description}\n",
"/description set {username} {email} - запомнить для игрока {username} адрес электронной почты {email}\n",
"/description delete {username} - удалить описание игрока {username}\n",
"/description list - показать список игроков и их описания\n"
];

//  Регистрируем команду `/description` без обработчика
var point = completer.addPlayerCommand( 'description' );

//  Регистрируем команду `/description help` и ее обрабочтик как команду для чата клиента
point.addComplete('help', cmd_help );

//  Регистрируем команду `/description list` и ее обрабочтик как команду для чата клиента
point.addComplete('list', cmd_list );


//  Регистрируем команду `/description set {username}` без обработчика.
//  Заметьте, что третим аргументом передана функция, userlist_to_autocomlete. 
//  Она возвращает ассоциативный массив, ключи которого будут внесены в список автозавершения для команды `/description set`
//  Не рационально выводить всех онлайн и офлайн игроков как автодополнение команды `/description set`.
//  Но мы делаем это для демонстрации возможностей модуля `last/completer`.
// Тег `@any` сопаставится с любым вводом после команды `/description` добавив в список автодополнений уже введенные символы.
// В нашем случае он будет сопоставлятся с введенными именами пользователей, включая пользователей оффлайн.
var point_set = point.addComplete('set', undefined, userlist_to_autocomlete )
           .addComplete('@any');


  // Регистрируем команду `/description set {username} {email}` и устанавливаем для нее обработчик. 
  point_set.addComplete('@re/(\\w+([-+.\']\\w+)*@\\w+([-.]\\w+)*\\.\\w+([-.]\\w+)*)/', cmd_set_email);
  //point_set.addComplete('@re/test\@gmail\.com/', cmd_set_email);

  // Регистрируем команду `/description set {username} {description}` и устанавливаем для нее обработчик. 
  point_set.addComplete('@any', cmd_set_description);



//  Регистрируем команду `/description delete {username}`. 
//  Мы могли зарегестрировать обработчики команды `/description delete {username}` так, как это сделано для команды `/description set {username} ...`.
//  Но для демонстрации возможностей я сделал это так:
var point_delete = point.addComplete('delete');

  // тег `@user` добавит в список автодополнений для команды `/description delete` всех пользователей онлайн.
  point_delete.addComplete('@user',cmd_delete); 
    // Для возможности указать ники офлайн игроков после команды `/description delete` используем тег `@any`.
    // Обратите внимание, на то, что ники офлайн игроков не будут автодополнятся. 
    // А так же на то, что что тег `@any` сопоставляется с любым вводом.
    // Поэтому используем его последним в цепочке автодополнений для команды `/description delete`.
  point_delete.addComplete('@any',cmd_delete);




// функция-обработчик для команд `/description` и `/description help`
// в `params[0]` будет `description`
// в `params[1]` будет `help`
function cmd_help(params, sender){
  echo(sender, help_messages);
}

// функция-обработчик для команды `/description list`
// в `params[0]` будет `description`
// в `params[1]` будет `list`
function cmd_list(params, sender){
  var description_msg = ["Список пользователей для которых есть description:"];
  for(var name in store ){
    str = name + " - ";
    if( store[name].email )
      str += "<"+store[name].email+"> ";
    if( store[name].info )
      str += store[name].info;
    str+="\n";
    description_msg.push(str);
  }

  echo(sender, description_msg);
}

// функция-обработчик для команды `/description set {username} {email}`
// в `params[0]` будет `description`
// в `params[1]` будет `{set}`
// в `params[2]` будет `{username}`
// в `params[3]` будет `{email}`
function cmd_set_email(params, sender){
  var name = params[2];
  var email = params[3];
  if( !store[name] )
    store[name] = {};
  store[name].email = email;
  echo(sender, "адрес электронной почты <"+email+"> успешно внесен в описание пользователя "+name);
}

// функция-обработчик для команды `/description set {username} {you description}`
// в `params[0]` будет `description`
// в `params[1]` будет `{set}`
// в `params[2]` будет `{username}`
// в `params[3]` будет `{you description}`
function cmd_set_description(params, sender){
  params.shift();
  params.shift();
  var name = params.shift();
  var info = params.join(" ");
  if( !store[name] )
    store[name] = {};
  store[name].info = info;
  echo(sender, "информация успешно внесена в описание пользователя "+name);
}

// функция-обработчик для команды `/description delete {username}`
// в `params[0]` будет `description`
// в `params[1]` будет `delete`
// в `params[2]` будет `username`
function cmd_delete(params, sender){
  var name = params[2];
  if( store[name] ){
    delete store[name];
    echo(sender, "описание для пользователя "+name+" успешно удалено");
  }else{
    echo(sender, "отсутствует описание для пользователя "+name);  
  }
  
}

// функция возвращает ассоциативный массив, ключи - ники всех зарегестрированных на сервере пользователей
function userlist_to_autocomlete(sender,patern){
  var result = {};
  var users = org.bukkit.Bukkit.getOfflinePlayers();
  for(var user in users){
    var name = users[user].name;
    result[name] = true;
  }
  return result;
}
```