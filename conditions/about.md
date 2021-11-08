# Об условиях

### Использование в других структурах
Во всех JSON-объектах, где поле называется `conditions` всегда подразумевается, что будет использована именно эта структура данных. 
Их можно задать для завершения главы, для выборочного показа кнопок в диалоге, а также при использовании очереди диалогов.

### Описание структуры данных
Данная структура данных имеет вид массива строк, где в строках использованы специальные команды условий.

### Как это работает
Все условия проверяются по порядку и если хоть одно условие было не выполнено, то какое-то действие не произойдет(зависит от контекста использования условий).

*Примечание: Если список условий будет пуст, то это означает, что все условия были соблюдены.*

### Примеры использования
```json
{
  "conditions": [
    "paragraph test",
    "not_completed test2",
    "day"
  ]
}
```
Пустой список условий
```json
{
  "conditions": []
}
```

# Список стандартных условий
Ниже представлен список всех стандартных условий:
<!-- - [](.md) -->

----------
# Создание собственных условий
Для создания собственного условия вам потребуется наследовать класс условия от класса `Condition`.

```java
public class DayCondition extends Condition {

    public DayCondition() {
        super("day"); //В конструктор суперкласса передается имя команды
    }

    @Override
    public Result handle(PlayerData playerData, String[] args) throws IndexOutOfBoundsException {
        int time = playerData.getPlayer().getLevel().getTime();

        return new Result("Дождаться дня", 1000 < time && time < 12000);
    }

}
```

Обработка результата идет в методе `handle`. Данный метод должен возвращать объект класса `Result` с результатом проверки данного условия.
Первый аргумент конструктора данного класса содержит текст условия, а второй аргумент - результат выполнения типа `boolean`.

Далее нам нужно зарегистрировать данное условие в менеджере условий, а также регистрация условия должна происходить при загрузке плагина(в методе `onLoad` в главном классе плагина).
```java
@Override
public void onLoad(){
    ConditionManager conditionManager = Expo.getInstance().getConditionManager();
    
    conditionManager.registerCondition(new DayCondition());
}
```