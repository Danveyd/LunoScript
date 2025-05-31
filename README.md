# LunoScript - Встраиваемый скриптовый язык для Android (Kotlin/Java)

LunoScript - это легковесный, интерпретируемый скриптовый язык с динамической типизацией, разработанный для встраивания в Android-приложения, написанные на Kotlin или Java. Он позволяет выполнять пользовательский код, управлять объектами приложения и расширять функциональность без перекомпиляции основного кода. Синтаксис LunoScript вдохновлен Kotlin, что делает его интуитивно понятным для Kotlin-разработчиков.

## Особенности

*   **Динамическая типизация:** Типы переменных определяются во время выполнения.
*   **Kotlin-подобный синтаксис:** Привычные конструкции для объявления переменных, условных операторов, циклов и функций.
*   **Встраиваемость:** Легко интегрируется в существующие Android-проекты через `LunoScriptEngine`.
*   **Расширяемость:** Позволяет определять "нативные" функции на Kotlin/Java, которые могут быть вызваны из LunoScript.
*   **Работа с объектами:** Поддержка списков, объектов (карт), а также базовых классов и функций, определенных в LunoScript.
*   **Взаимодействие с Kotlin/Java:** Возможность передавать Kotlin/Java объекты в LunoScript (как `NativeObject`) и вызывать их методы (через специально определенные нативные функции).

## Основы языка LunoScript

### 1. Комментарии

Однострочные комментарии начинаются с `//`.

```Luno
// Это однострочный комментарий
var x = 10; // Это тоже комментарий
```

### 2. Переменные

Переменные объявляются с помощью ключевого слова `var`. Тип переменной определяется значением, которое ей присваивается.

```Luno
var message; // Переменная без начального значения (будет `null`)
var count = 10; // count - это Number
var name = "Luno"; // name - это String
var isActive = true; // isActive - это Boolean

count = count + 5;
name = "LunoScript";
```

### 3. Типы данных

LunoScript поддерживает следующие основные типы данных:

*   **`Number`**: Числа (хранятся как `Double` в Kotlin).
    ```Luno
    var pi = 3.14;
    var integer = 100;
    ```
*   **`String`**: Строки. Строковые литералы заключаются в двойные кавычки.
    ```Luno
    var greeting = "Hello, World!";
    var multiline = "Это\nмногострочная\nстрока.";
    ```
*   **`Boolean`**: Логические значения `true` и `false`.
    ```Luno
    var isReady = true;
    var hasErrors = false;
    ```
*   **`Null`**: Специальное значение `null`, представляющее отсутствие значения.
    ```Luno
    var data = null;
    ```
*   **`List`**: Упорядоченные коллекции значений (аналог массивов/списков).
    ```Luno
    var numbers = [1, 2, 3, 4, 5];
    var mixedList = [10, "строка", true, null, [100, 200]];
    print(numbers[0]); // Выведет: 1
    numbers[1] = 20;
    ```
*   **`Object`**: Коллекции пар "ключ-значение" (аналог карт/словарей). Ключи являются строками. Используются также для представления экземпляров классов.
    ```Luno
    var person = {"name": "Alex", "age": 30, "isDeveloper": true};
    print(person.name); // Выведет: Alex
    print(person["age"]); // Выведет: 30
    person.city = "New York"; // Добавление нового поля
    ```
*   **`Function`**: Функции, определенные пользователем.
*   **`Class`**: Классы, определенные пользователем.
*   **`NativeObject`**: Специальный тип для представления "сырых" Kotlin/Java объектов, переданных в LunoScript.
*   **`NativeCallable`**: Специальный тип для представления "сырых" Kotlin/Java функций, доступных из LunoScript.

### 4. Операторы

#### Арифметические операторы
*   `+` (сложение, конкатенация строк, конкатенация списков)
*   `-` (вычитание, унарный минус)
*   `*` (умножение, повторение строк, повторение списков)
*   `/` (деление)
*   `%` (остаток от деления)

```Luno
var sum = 10 + 5; // 15
var fullName = "Luno" + "Script"; // "LunoScript"
var repeated = "abc" * 3; // "abcabcabc"
var a = [1,2]; var b = [3,4]; var c = a + b; // [1,2,3,4]
var result = (100 - 20) / 2 * 3; // 120
```

#### Операторы присваивания
*   `=` (присвоить)
*   `+=`, `-=`, `*=`, `/=`, `%=` (составное присваивание)

```Luno
var x = 10;
x += 5; // x теперь 15
```

#### Операторы сравнения
*   `==` (равно)
*   `!=` (не равно)
*   `>` (больше)
*   `<` (меньше)
*   `>=` (больше или равно)
*   `<=` (меньше или равно)

#### Логические операторы
*   `&&` (логическое И)
*   `||` (логическое ИЛИ)
*   `!` (логическое НЕ)

```Luno
var age = 25;
var hasLicense = true;
if (age >= 18 && hasLicense) {
    print("Можно водить");
}
if (!hasLicense) {
    print("Нет прав");
}
```

### 5. Управляющие конструкции

#### `if / else`
```Luno
var temperature = 25;
if (temperature > 30) {
    print("Жарко!");
} else if (temperature > 20) {
    print("Тепло.");
} else {
    print("Прохладно.");
}
```

#### `while` цикл
```Luno
var i = 0;
while (i < 3) {
    print("i = " + i);
    i = i + 1;
}
```

#### `for...in` цикл
Используется для итерации по элементам списка или символам строки.
```Luno
var fruits = ["яблоко", "банан", "апельсин"];
for (fruit in fruits) {
    print(fruit);
}

for (char in "LUNO") {
    print("Символ: " + char);
}
```

#### `switch`
Работает аналогично `when` в Kotlin, но без автоматического "проваливания". `break` не обязателен для прекращения выполнения `case`.
```Luno
var day = "Monday";
switch (day) {
    case "Monday":
        print("Начало недели");
    case "Friday":
        print("Почти выходные!");
    case "Saturday", "Sunday": // Несколько значений для одного case
        print("Выходной!");
    default:
        print("Будний день.");
}
```

#### `break` и `continue`
*   `break;`: Прерывает выполнение текущего цикла (`while`, `for`) или `switch`.
*   `continue;`: Переходит к следующей итерации текущего цикла (`while`, `for`).

### 6. Функции

Функции объявляются с помощью ключевого слова `fun`.
```Luno
fun greet(name) {
    var message = "Привет, " + name + "!";
    return message;
}

var myGreeting = greet("Пользователь Luno");
print(myGreeting); // Выведет: Привет, Пользователь Luno!

fun doNothing() {
    // Эта функция ничего не возвращает явно (вернет null)
}
print(doNothing()); // Выведет: null
```

### 7. Классы (базовая поддержка)

Классы объявляются с помощью ключевого слова `class`. Конструктор определяется методом `init`. `this` используется для доступа к членам экземпляра.
```Luno
class Point {
    fun init(x, y) { // Конструктор
        this.x = x;
        this.y = y;
        print("Точка создана: " + this.x + ", " + this.y);
    }

    fun move(dx, dy) {
        this.x = this.x + dx;
        this.y = this.y + dy;
    }

    fun display() {
        return "Point(" + this.x + ", " + this.y + ")";
    }
}

var p1 = Point(10, 20); // Создание экземпляра, вызов init
print(p1.display());     // Вызов метода
p1.move(5, -5);
print("p1 после перемещения: " + p1.display());
print("Координата X точки p1: " + p1.x);

p1.color = "red"; // Поля можно добавлять динамически
print("Цвет точки p1: " + p1.color);
```

### 8. Доступ к элементам

*   **Списки:** `myList[index]`
*   **Объекты (карты):** `myObject.property` или `myObject["propertyName"]`
*   **Строки:** `myString[index]` (для получения символа)

### 9. Точка с запятой (`;`)
В большинстве случаев точка с запятой в конце инструкций опциональна. Но рекомендуется ее ставить, дабы избежать недоразумений

## Встроенные (нативные) функции

LunoScript предоставляет набор встроенных функций для взаимодействия с окружением и выполнения общих задач.

```Luno
// Формат описания:
// FunctionName(parameter1, parameter2?) -> ReturnType
//   Описание функции.
//   parameter1: Тип - Описание параметра.
//   parameter2 (опционально): Тип - Описание параметра.
//   Возвращает: Тип - Описание возвращаемого значения.
```

---

### Общие функции

*   **`print(value1, value2, ...)`**
    *   Выводит одно или несколько значений в лог Android (Log.d с тегом "LunoScript"). Значения разделяются пробелом.
    *   `value`: Любой тип LunoScript.
    *   Возвращает: `Null`.
    ```Luno
    print("Сообщение", 123, true);
    ```

*   **`len(collection)`**
    *   Возвращает длину строки, количество элементов в списке или количество полей в объекте.
    *   `collection`: `String`, `List` или `Object`.
    *   Возвращает: `Number`.
    ```Luno
    print(len("hello")); // 5
    print(len([1, 2, 3])); // 3
    print(len({"a":1, "b":2})); // 2
    ```

*   **`typeof(value)`**
    *   Возвращает строку, представляющую тип значения LunoScript.
    *   `value`: Любой тип LunoScript.
    *   Возвращает: `String` (например, "String", "Number", "List", "Object", "Boolean", "Null", "Function", "Class", "NativeObject", "NativeCallable").
    ```Luno
    print(typeof(10)); // "Number"
    print(typeof("abc")); // "String"
    print(typeof(null)); // "Null"
    ```

*   **`String(value)`**
    *   Преобразует значение в его строковое представление.
    *   `value`: Любой тип LunoScript.
    *   Возвращает: `String`.
    ```Luno
    var num = 123;
    var strNum = String(num); // "123"
    ```

*   **`Number(value)`**
    *   Преобразует значение в число.
    *   `value`: `Number`, `String` (пытается распарсить), `Boolean` (`true` -> 1, `false` -> 0), `Null` (-> 0). Для других типов возвращает `NaN`.
    *   Возвращает: `Number`.
    ```Luno
    var numStr = "42.5";
    var num = Number(numStr); // 42.5
    print(Number(true)); // 1.0
    ```

*   **`Boolean(value)`**
    *   Преобразует значение в его булево представление (истинность).
    *   `value`: Любой тип LunoScript.
    *   Возвращает: `Boolean`.
    *   Ложными (`false`) считаются: `null`, `false`, число `0`, пустая строка `""`, пустой список `[]`. Все остальное считается истинным (`true`).
    ```Luno
    print(Boolean(0)); // false
    print(Boolean("hello")); // true
    print(Boolean([])); // false
    ```

*   **`throw(message)`**
    *   Генерирует ошибку времени выполнения LunoScript с указанным сообщением.
    *   `message`: `String` - Сообщение об ошибке.
    *   (Не возвращает значение, прерывает выполнение).
    ```Luno
    // throw("Это кастомная ошибка!");
    ```

### Функции для Android

*   **`GetAppContext()`**
    *   Возвращает нативный объект Android `Context` приложения.
    *   Возвращает: `NativeObject` (содержащий `android.content.Context`).
    *   *Примечание: Этот объект обычно используется как аргумент для других нативных функций, требующих контекст.*

*   **`MakeToast(message)`**
    *   Отображает короткое всплывающее сообщение (Toast) на экране.
    *   `message`: `String` - Текст сообщения.
    *   Возвращает: `Null`.
    ```Luno
    MakeToast("Привет из LunoScript!");
    ```

### Числовые функции (Парсинг и Проверки)

*   **`parseInt(string, radix?)`**
    *   Преобразует строку в целое число по указанному основанию.
    *   `string`: `String` - Строка для парсинга.
    *   `radix` (опционально): `Number` - Основание системы счисления (от 2 до 36). По умолчанию 10.
    *   Возвращает: `Number` (целое число или `NaN`, если парсинг не удался).
    ```Luno
    print(parseInt("101", 2)); // 5.0 (двоичное)
    print(parseInt("FF", 16)); // 255.0 (шестнадцатеричное)
    print(parseInt("42")); // 42.0
    print(parseInt("hello")); // NaN
    ```

*   **`parseFloat(value)`**
    *   Преобразует значение (обычно строку) в число с плавающей точкой. Пытается имитировать поведение `parseFloat` из JavaScript.
    *   `value`: `String` или `Number`. Если `Number`, возвращает его же. Другие типы сначала преобразуются в строку.
    *   Возвращает: `Number` (число или `NaN`).
    ```Luno
    print(parseFloat("3.14")); // 3.14
    print(parseFloat("  -10.5ignored")); // -10.5
    print(parseFloat("Infinity")); // Infinity
    print(parseFloat("text")); // NaN
    ```

*   **`isNaN(value)`**
    *   Проверяет, является ли значение `NaN` (Not-a-Number).
    *   `value`: `Number`. Для других типов всегда возвращает `false`.
    *   Возвращает: `Boolean`.
    ```Luno
    print(isNaN(0/0)); // true
    print(isNaN(10));  // false
    print(isNaN("text")); // false (т.к. "text" не Number)
    ```

*   **`isFinite(value)`**
    *   Проверяет, является ли значение конечным числом (не `Infinity`, не `-Infinity`, не `NaN`).
    *   `value`: `Number`. Для других типов всегда возвращает `false`.
    *   Возвращает: `Boolean`.
    ```Luno
    print(isFinite(100)); // true
    print(isFinite(1/0)); // false (Infinity)
    print(isFinite(Number("NaN"))); // false
    ```

### Математические функции

*   **`currentTimeMillis()`**
    *   Возвращает текущее время в миллисекундах (относительно эпохи Unix).
    *   Возвращает: `Number`.

*   **`sqrt(number)`**
    *   Вычисляет квадратный корень числа.
    *   `number`: `Number`.
    *   Возвращает: `Number` (или `NaN` для отрицательных чисел).

*   **`abs(number)`**
    *   Возвращает абсолютное значение (модуль) числа.
    *   `number`: `Number`.
    *   Возвращает: `Number`.

*   **`round(number)`**
    *   Округляет число до ближайшего целого.
    *   `number`: `Number`.
    *   Возвращает: `Number`.

*   **`floor(number)`**
    *   Округляет число вниз до ближайшего целого.
    *   `number`: `Number`.
    *   Возвращает: `Number`.

*   **`ceil(number)`**
    *   Округляет число вверх до ближайшего целого.
    *   `number`: `Number`.
    *   Возвращает: `Number`.

*   **`random()`**
    *   Возвращает случайное число с плавающей точкой в диапазоне от 0.0 (включительно) до 1.0 (исключительно).
    *   Возвращает: `Number`.

*   **`min(number1, number2, ...)`**
    *   Возвращает наименьшее из переданных чисел.
    *   `numberX`: `Number`. Требуется хотя бы один аргумент.
    *   Возвращает: `Number`.

*   **`max(number1, number2, ...)`**
    *   Возвращает наибольшее из переданных чисел.
    *   `numberX`: `Number`. Требуется хотя бы один аргумент.
    *   Возвращает: `Number`.

*   **`pow(base, exponent)`**
    *   Возводит число `base` в степень `exponent`.
    *   `base`: `Number`.
    *   `exponent`: `Number`.
    *   Возвращает: `Number`.

*   **Константы:**
    *   `PI`: Число Пи (приблизительно 3.14159).
    *   `E`: Число Эйлера (основание натурального логарифма, приблизительно 2.71828).

### Функции для работы со строками

*   **`toUpperCase(string)`**
    *   Преобразует строку в верхний регистр.
    *   `string`: `String`.
    *   Возвращает: `String`.

*   **`toLowerCase(string)`**
    *   Преобразует строку в нижний регистр.
    *   `string`: `String`.
    *   Возвращает: `String`.

*   **`trim(string)`**
    *   Удаляет пробелы в начале и конце строки.
    *   `string`: `String`.
    *   Возвращает: `String`.

*   **`startsWith(string, prefix)`**
    *   Проверяет, начинается ли строка `string` с подстроки `prefix`.
    *   `string`: `String`.
    *   `prefix`: `String`.
    *   Возвращает: `Boolean`.

*   **`endsWith(string, suffix)`**
    *   Проверяет, заканчивается ли строка `string` подстрокой `suffix`.
    *   `string`: `String`.
    *   `suffix`: `String`.
    *   Возвращает: `Boolean`.

*   **`stringContains(string, substring)`**
    *   Проверяет, содержит ли строка `string` подстроку `substring`.
    *   `string`: `String`.
    *   `substring`: `String`.
    *   Возвращает: `Boolean`.

*   **`replace(string, oldValue, newValue)`**
    *   Заменяет все вхождения подстроки `oldValue` на `newValue` в строке `string`.
    *   `string`: `String`.
    *   `oldValue`: `String`.
    *   `newValue`: `String`.
    *   Возвращает: `String`.

*   **`split(string, delimiter)`**
    *   Разбивает строку `string` на список подстрок, используя `delimiter` в качестве разделителя.
    *   `string`: `String`.
    *   `delimiter`: `String`.
    *   Возвращает: `List` из `String`.
    ```Luno
    var parts = split("яблоко,банан,апельсин", ",");
    // parts будет ["яблоко", "банан", "апельсин"]
    ```

*   **`substring(string, startIndex, endIndex?)`**
    *   Возвращает подстроку из `string`, начиная с `startIndex` (включительно) до `endIndex` (исключительно).
    *   `string`: `String`.
    *   `startIndex`: `Number` (целое).
    *   `endIndex` (опционально): `Number` (целое). Если не указан, то до конца строки.
    *   Возвращает: `String`.
    ```Luno
    var text = "Hello Luno";
    print(substring(text, 0, 5)); // "Hello"
    print(substring(text, 6));    // "Luno"
    ```

### Функции для работы со списками

*   **`listPush(list, element1, element2, ...)`**
    *   Добавляет один или несколько элементов в конец списка `list`. Изменяет исходный список.
    *   `list`: `List`.
    *   `elementX`: Любой тип LunoScript.
    *   Возвращает: `Number` (новая длина списка).

*   **`listPop(list)`**
    *   Удаляет и возвращает последний элемент из списка `list`. Изменяет исходный список.
    *   `list`: `List`.
    *   Возвращает: Удаленный элемент или `Null`, если список пуст.

*   **`listShift(list)`**
    *   Удаляет и возвращает первый элемент из списка `list`. Изменяет исходный список.
    *   `list`: `List`.
    *   Возвращает: Удаленный элемент или `Null`, если список пуст.

*   **`listUnshift(list, element1, element2, ...)`**
    *   Добавляет один или несколько элементов в начало списка `list`. Изменяет исходный список.
    *   `list`: `List`.
    *   `elementX`: Любой тип LunoScript.
    *   Возвращает: `Number` (новая длина списка).

*   **`listJoin(list, separator?)`**
    *   Объединяет все элементы списка `list` в одну строку.
    *   `list`: `List`.
    *   `separator` (опционально): `String` - Разделитель между элементами. По умолчанию ",".
    *   Возвращает: `String`.
    ```Luno
    var items = ["a", "b", "c"];
    print(listJoin(items));       // "a,b,c"
    print(listJoin(items, "-"));  // "a-b-c"
    ```

*   **`listSlice(list, startIndex, endIndex?)`**
    *   Возвращает неглубокую копию части списка `list` от `startIndex` (включительно) до `endIndex` (исключительно). Не изменяет исходный список.
    *   `list`: `List`.
    *   `startIndex`: `Number` (целое). Отрицательные значения отсчитываются с конца.
    *   `endIndex` (опционально): `Number` (целое). Если не указан, то до конца списка. Отрицательные значения отсчитываются с конца.
    *   Возвращает: Новый `List`.
    ```Luno
    var original = [0, 1, 2, 3, 4];
    var sliced = listSlice(original, 1, 3); // [1, 2]
    var slicedToEnd = listSlice(original, 2); // [2, 3, 4]
    print(original); // [0, 1, 2, 3, 4] (не изменен)
    ```

*   **`listReverse(list)`**
    *   Обращает порядок элементов в списке `list` **на месте** (изменяет исходный список).
    *   `list`: `List`.
    *   Возвращает: Тот же измененный `List`.

### Функции для работы с объектами

*   **`ObjectKeys(object)`**
    *   Возвращает список строк, представляющих ключи (имена свойств) объекта.
    *   `object`: `Object`.
    *   Возвращает: `List` из `String`.
    ```Luno
    var user = {"name": "Kate", "age": 28};
    var keys = ObjectKeys(user); // ["name", "age"] (порядок может варьироваться)
    ```

*   **`ObjectValues(object)`**
    *   Возвращает список значений свойств объекта.
    *   `object`: `Object`.
    *   Возвращает: `List` (элементы могут быть разных типов).
    ```Luno
    var user = {"name": "Kate", "age": 28};
    var values = ObjectValues(user); // ["Kate", 28.0] (порядок соответствует ObjectKeys)
    ```

*   **`ObjectHasProperty(object, key)`**
    *   Проверяет, есть ли у объекта `object` свойство с ключом `key`.
    *   `object`: `Object` или `NativeObject` (если это `Map`).
    *   `key`: `String`.
    *   Возвращает: `Boolean`.

### Функции JSON (базовая поддержка)

*   **`JSON_parse(jsonString)`**
    *   Преобразует строку `jsonString` в формате JSON в объект или список LunoScript.
    *   `jsonString`: `String`.
    *   Возвращает: `Object`, `List` или другой примитивный тип LunoScript (или `Null` при ошибке).
    *   *Примечание: Текущая реализация очень базовая и может не поддерживать все возможности JSON. Для сложного JSON рекомендуется обрабатывать его на стороне Kotlin/Java.*

*   **`JSON_stringify(value)`**
    *   Преобразует значение LunoScript в строку формата JSON.
    *   `value`: Любой тип LunoScript.
    *   Возвращает: `String`.
    *   *Примечание: Функции и сложные нативные объекты могут не сериализоваться корректно.*

### Функции для работы с "формулами" (специфично для вашего проекта)

*   **`Formula(formulaString)`**
    *   Создает нативный объект "формулы" из строки.
    *   `formulaString`: `String` - Строковое представление формулы.
    *   Возвращает: `NativeObject` (содержащий объект `Formula`).

*   **`RegisterFormula(uniqueName, displayName, defaultParamValues, defaultParamTypes, jsCode)`**
    *   Регистрирует новую "кастомную формулу" в системе.
    *   `uniqueName`: `String` - Уникальное имя для идентификации формулы.
    *   `displayName`: `String` - Отображаемое имя формулы.
    *   `defaultParamValues`: `List` из `String` - Список строковых значений по умолчанию для параметров. Количество параметров формулы определяется размером этого списка.
    *   `defaultParamTypes`: `List` из `String` - Список строк, представляющих типы параметров (например, "STRING", "NUMBER"). Должен иметь тот же размер, что и `defaultParamValues`.
    *   `jsCode`: `String` - JavaScript код, реализующий логику формулы.
    *   Возвращает: `Null`.
    ```Luno
    RegisterFormula(
        "myConcat",
        "My Concatenator",
        ["prefix_", "_suffix"], // Значения по умолчанию
        ["STRING", "STRING"],    // Типы
        "return String(p[0]) + 'DATA' + String(p[1]);"
    );
    ```

### Функции для работы с локальными переменными (специфично для NewCatroid)

*   **`SetLocalVar(name, value)`**: Устанавливает значение локальной переменной.
*   **`GetLocalVar(name)`**: Получает значение локальной переменной.
*   **`DeleteLocalVar(name)`**: Удаляет локальную переменную.
*   **`DeleteLocalVars()`**: Удаляет все локальные переменные.
*   **`GetLocalVarName(index)`**: Получает имя локальной переменной по индексу.
*   **`GetLocalVarValue(index)`**: Получает значение локальной переменной по индексу.

### Функции для взаимодействия с проектом NewCatroid (Project, Sprite, Look, Script)

*Функции этой категории возвращают или принимают `NativeObject`, представляющие соответствующие Kotlin/Java классы из настоящего проекта. Для работы с этими объектами (чтение полей, вызов методов) вам, скорее всего, понадобятся дополнительные нативные функции, специфичные для каждого типа объекта, либо вы будете использовать их как "непрозрачные" указатели для передачи в другие нативные функции.*

*   **`GetScope()` -> `NativeObject` (Scope)**: Возвращает текущий объект "Scope".
*   **`GetSprite(scope)` -> `NativeObject` (Sprite)**: Получает спрайт из указанного `scope`.
*   **`GetProject(scope)` -> `NativeObject` (Project)**: Получает проект из указанного `scope`.
*   **`ProjectGetDirectory(project)` -> `String`**
*   **`ProjectSetDirectory(project, pathString)` -> `Null`**
*   **`ProjectGetSceneList(project)` -> `List` из `NativeObject` (Scene)**
*   **`ProjectGetSceneNames(project)` -> `List` из `String`**
*   **`ProjectAddScene(project, sceneNativeObject)` -> `Null`**
*   **`ProjectRemoveScene(project, sceneNativeObject)` -> `Null`**
*   **`ProjectHasScene(project)` -> `Boolean`**
*   **`ProjectGetDefaultScene(project)` -> `NativeObject` (Scene)**
*   **`ProjectGetUserVariables(project)` -> `List` из `NativeObject` (UserVariable)**
*   **`ProjectGetUserVariablesCopy(project)` -> `List` из `NativeObject` (UserVariable)**
*   **`ProjectGetUserVariable(project, nameString)` -> `NativeObject` (UserVariable)**
*   **`ProjectAddUserVariable(project, userVariableNativeObject)` -> `Null`**
*   **`ProjectRemoveUserVariable(project, nameString)` -> `Null`**
*   **`UserVariable(nameString?, valueString?)` -> `NativeObject` (UserVariable)**: Создает новый объект UserVariable.
*   **`ProjectGetUserLists(project)` -> `List` из `NativeObject` (UserList)**
*   **`ProjectGetUserListsCopy(project)` -> `List` из `NativeObject` (UserList)**
*   **`ProjectGetUserList(project, nameString)` -> `NativeObject` (UserList)**
*   **`ProjectAddUserList(project, userListNativeObject)` -> `Null`**
*   **`ProjectRemoveUserList(project, nameString)` -> `Null`**
*   **`UserList(nameString?, initialValuesList?)` -> `NativeObject` (UserList)**: Создает новый UserList. `initialValuesList` - это `List` из LunoScript.
*   **`ProjectResetUserData(project)` -> `Null`**
*   **`ProjectGetSpriteListWithClones(project)` -> `List` из `NativeObject` (Sprite)**
*   **`ProjectGetName(project)` -> `String`**
*   **`ProjectSetName(project, nameString)` -> `Null`**
*   **`ProjectGetDescription(project)` -> `String`**
*   **`ProjectSetDescription(project, descriptionString)` -> `Null`**
*   **`ProjectGetNotesAndCredits(project)` -> `String`**
*   **`ProjectSetNotesAndCredits(project, notesString)` -> `Null`**
*   **`ProjectGetCatrobatLanguageVersion(project)` -> `String` (представление Double)**
*   **`ProjectSetCatrobatLanguageVersion(project, versionString)` -> `Null`** (ожидает строку, конвертируемую в Double)
*   **`ProjectGetXmlHeader(project)` -> `NativeObject` (XmlHeader)**
*   **`ProjectSetXmlHeader(project, xmlHeaderNativeObject)` -> `Null`**
*   **`ProjectGetFilesDir(project)` -> `String` (путь)**
*   **`ProjectGetLibsDir(project)` -> `String` (путь)**
*   **`ProjectGetFile(project, fileNameString)` -> `String` (путь к файлу)**
*   **`ProjectGetLib(project, libNameString)` -> `String` (путь к библиотеке)**
*   **`ProjectCheckExtension(project, extensionNameString, versionString)` -> `String`**
*   **`ProjectAddFile(project, fileNativeObjectOrPathString)` -> `Null`**
*   **`ProjectDeleteFile(project, fileOrPathString)` -> `Null`**
*   **`File(pathString, childPathString?)` -> `NativeObject` (java.io.File)**: Создает объект файла.
*   **`FilePath(fileNativeObject)` -> `String`**: Возвращает абсолютный путь файла.
*   **`Rectangle(xNum, yNum, widthNum, heightNum)` -> `NativeObject` (Rectangle)**: Создает объект Rectangle.
*   **`ProjectGetScreenRectangle(project)` -> `NativeObject` (Rectangle)**
*   **`ProjectGetTags(project)` -> `List` из `NativeObject` (String Tag)**
*   **`ProjectSetTags(project, listOfStringTags)` -> `Null`**
*   **`ProjectGetSceneByName(project, nameString)` -> `NativeObject` (Scene)**
*   **`ProjectGetSceneById(project, idString)` -> `NativeObject` (Scene)** (idString будет преобразован в Int)
*   **`SpriteGetName(spriteNativeObject)` -> `String`**
*   **`Sprite(nameString)` -> `NativeObject` (Sprite)**: Создает новый Sprite с именем.
*   **`Sprite(existingSpriteNativeObject, sceneNativeObject)` -> `NativeObject` (Sprite)**: Создает Sprite на основе существующего в сцене.
*   **`Look(spriteNativeObject)` -> `NativeObject` (Look)**: Создает новый Look для спрайта.
*   **`SpriteGetLook(spriteNativeObject)` -> `NativeObject` (Look)**
*   **`LookRemove(lookNativeObject)` -> `Null`** (предположительно, удаляет образ)
*   **`LookGetLookData(lookNativeObject)` -> `NativeObject` (LookData)**
*   **`LookGetLookData2(lookNativeObject)` -> `NativeObject` (LookData)** (вероятно, для второго слоя/состояния)
*   **`LookData(nameString, fileNativeObjectOrPathString)` -> `NativeObject` (LookData)**: Создает LookData.
*   **`LookDataGetName(lookDataNativeObject)` -> `String`** (должен быть `NativeObject` в аргументе, судя по реализации)
*   **`LookDataSetName(lookDataNativeObject, nameString)` -> `Null`**
*   **`LookDataGetFile(lookDataNativeObject)` -> `NativeObject` (File)**
*   **`LookDataSetFile(lookDataNativeObject, fileNativeObjectOrPathString)` -> `Null`**
*   **`LookSetLookData(lookNativeObject, lookDataNativeObject)` -> `Null`**
*   **`LookSetLookData2(lookNativeObject, lookDataNativeObject)` -> `Null`**
*   **`LookGetX(look)`/`Y`/`Width`/`Height`/`Rotation`/`Alpha`/`Brightness`/`Hue` -> `Number`**
*   **`LookGetVisible(look)`/`LookGetLookVisible(look)` -> `Boolean`**
*   **`LookSetX(look, value)`/`Y`/`Width`/`Height`/`Rotation`/`Alpha`/`Brightness`/`Hue`/`Visible`/`LookVisible` -> `Null`**
*   **`SpriteGetScriptList(sprite)` -> `List` из `NativeObject` (Script)**
*   **`SpriteGetLookList(sprite)` -> `List` из `NativeObject` (Look)**
*   **`SpriteGetSoundList(sprite)` -> `List` из `NativeObject` (Sound)** (Sound еще не описан)
*   **`SpriteGetUserVariables(sprite)` -> `List` из `NativeObject` (UserVariable)**
*   **`SpriteGetUserLists(sprite)` -> `List` из `NativeObject` (UserList)**
*   **`ActionFactory()` -> `NativeObject` (ActionFactory)**
*   **`SpriteGetActionFactory(sprite)` -> `NativeObject` (ActionFactory)**
*   **`SpriteIsClone(sprite)` -> `Boolean`**
*   **`SpriteOriginal(sprite)` -> `NativeObject` (Sprite)** (оригинал, если это клон)
*   **`SpriteIsGliding(sprite)` -> `Boolean`**
*   **`SpriteGetAllBricks(sprite)` -> `List` из `NativeObject` (Brick)**
*   **`SpriteGetUserVariable(sprite, nameString)` -> `NativeObject` (UserVariable)**
*   **`SpriteAddUserVariable(sprite, userVariableNativeObject)` -> `Null`**
*   **`SpriteAddUserList(sprite, userListNativeObject)` -> `Null`**
*   **`SpriteGetUserList(sprite, nameString)` -> `NativeObject` (UserList)**
*   **`SpriteAddScript(sprite, scriptNativeObject, indexNum?)` -> `Null`**
*   **`SpriteGetScript(sprite, indexNumString)` -> `NativeObject` (Script)**
*   **`SpriteRemoveAllScripts(sprite)` -> `Null`**
*   **`SpriteRemoveScript(sprite, scriptNativeObject)` -> `Null`**
*   **`SpriteIsBackground(sprite)` -> `Boolean`**
*   **`ScriptGetBrickList(scriptNativeObject)` -> `List` из `NativeObject` (Brick)**
*   **`ScriptRun(script, sprite, sequenceAction)` -> `Null`**
*   **`GetSequence(scope)` -> `NativeObject` (ScriptSequenceAction)**
*   **`ScriptSequenceAction(scriptNativeObject)` -> `NativeObject` (ScriptSequenceAction)**
*   **`ScriptAddBrick(script, brick, indexNum?)` -> `Null`**
*   **`ScriptRemoveBrick(script, brick)` -> `Null`**
*   **`ScriptGetBrick(script, indexNumString)` -> `NativeObject` (Brick)**
*   **`UserVariableGetValue(userVariable)` -> `String`** (представление значения)
*   **`UserVariableSetValue(userVariable, valueString)` -> `Null`**
*   **`Brick(brickTypeNameString, parametersList)` -> `NativeObject` (конкретный Brick)**: Создает экземпляр блока по его имени и списку параметров LunoScript.
*   **`RunBrick(brickNativeObject, spriteNativeObject, sequenceActionNativeObject)` -> `Null`**: Запускает блок (вызывает `addActionToSequence`).
