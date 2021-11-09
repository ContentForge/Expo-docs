# О кнопках диалога
Кнопки диалога применяются в диалогах(логино) для взаимодействия с NPC.

### Структура данных
Кнопка диалога - это JSON-объект, внутри которого описаны параметры кнопки диалога. Ниже приведена таблица с KV-парами данного JSON-объекта).

Ключ | Описание | Тип | Стандартное значение
--- | --- | --- | ---
text | Текст данной кнопки | string | ❌
action | Действие данной кнопки. Список всех действий кнопок находится в следующем разделе | string | "none"
conditions | Условия при выполнения которых данная кнопка будет видна игроку | string[] | []

*Все элементы у которого отсутствует стандартное значение обязательны для ввода

### Примеры кнопок диалога
```json
{
  "text": "Пока"
}
```
```json
{
  "text": "Как дела?",
  "action": "question Да нормально.",
  "conditions": [
    "day"
  ]
}
```

# Список стандартных кнопок диалога
Ниже представлен список всех стандартных кнопок диалога:
<!-- [](.md)(``) -->

# Создание собственной кнопки диалога
Для создания собственной кнопки диалога вам потребуется наследовать класс кнопки от класса `ButtonInstance`.
```java
public class DispatchCommandButtonInstance extends ButtonInstance {

    public DispatchCommandButtonInstance() {
        super("command"); //В конструктор суперкласса передаем id данной кнопки диалога
    }

    @Override
    public void handle(PlayerData playerData, DialogueSender sender, Dialogue dialogue, String text, String[] args) throws DialogueError {
        if(args.length == 0) throw new DialogueError("Неверное кол-во аргументов.");

        playerData.getPlayer().getServer().dispatchCommand(playerData.getPlayer(), String.join(" ", args));
    }

}
```
Здесь метод `handle` вызывается при нажиме кнопки игроком.

Также мы можем переопределить методы `getButtonIcon` для смены иконки кнопки и `convertButtonText` для того чтобы изменить формат текста кнопки.

После создания класса нам потребуется зарегистрировать данную кнопку в менеджере диалогов, а также регистрация кнопок должна происходить при загрузке плагина(в методе onLoad в главном классе плагина).
```java
@Override
public void onLoad(){
    DialogueManager dialogueManager = Expo.getInstance().getDialogueManager();
    
    dialogueManager.registerButtonInstance(new DispatchCommandButtonInstance());
}
```
