### Регулярные выражения

1. **Что такое регулярные выражения? Когда они могут быть полезны?**
   Регулярные выражения — это шаблоны для поиска и манипуляции текстами. Они могут быть полезны для проверки формата данных, поиска и замены текстов, валидирования пользовательского ввода и многого другого.

2. **Какие есть способы создания инстанса регулярного выражения?**
   - **Литерал регулярного выражения:**
     ```javascript
     const regex = /pattern/;
     ```
   - **Конструктор `RegExp`:**
     ```javascript
     const regex = new RegExp('pattern');
     ```

3. **Что такое флаги? Зачем нужны? Какие бывают?**
   Флаги изменяют поведение регулярного выражения:
   - **`g`** (глобальный): Ищет все вхождения, а не только первое.
   - **`i`** (игнорировать регистр): Не учитывает регистр при поиске.
   - **`m`** (многострочный): Включает режим многострочного поиска.
   - **`s`** (дот соответствует новой строке): Точка `.` соответствует любому символу, включая новую строку.
   - **`u`** (unicode): Поддержка unicode.
   - **`y`** (sticky): Поиск начинается с текущей позиции в строке.

4. **Какие методы есть у инстансов регулярных выражений?**
   - **`exec(str)`**: Выполняет поиск по строке и возвращает результат.
   - **`test(str)`**: Проверяет, соответствует ли строка регулярному выражению.
   - **`toString()`**: Возвращает строковое представление регулярного выражения.

5. **Какие методы есть у строк, которые позволяют работать с регулярными выражениями?**
   - **`match(regex)`**: Находит все совпадения с регулярным выражением.
   - **`replace(regex, newSubstr)`**: Заменяет совпадения с регулярным выражением.
   - **`search(regex)`**: Ищет совпадение с регулярным выражением и возвращает его позицию.
   - **`split(regex)`**: Разделяет строку по совпадениям с регулярным выражением.

6. **Что такое квантификаторы? Как работают квантификаторы `{}`, `?`, `*`, `+`?**
   Квантификаторы определяют количество повторений:
   - **`{n}`**: Точно `n` повторений.
   - **`{n,}`**: Не менее `n` повторений.
   - **`{n,m}`**: От `n` до `m` повторений.
   - **`?`**: Ноль или одно повторение.
   - **`*`**: Ноль или более повторений.
   - **`+`**: Одно или более повторений.

   **Жадные и ленивые квантификаторы:**
   - **Жадные:** Захватывают как можно больше текста (например, `.*`).
   - **Ленивые:** Захватывают как можно меньше текста (например, `.*?`).

7. **Что такое набор `[abc]` и диапазон `[a-c]` символов? Когда они могут быть полезны?**
   - **Набор:** Соответствует любому одному из перечисленных символов (`[abc]` соответствует `a`, `b` или `c`).
   - **Диапазон:** Соответствует любому символу из диапазона (`[a-c]` соответствует `a`, `b` или `c`).

   **Исключающий набор:** Набор, который исключает определенные символы (`[^abc]` соответствует любому символу, кроме `a`, `b` и `c`).

8. **Для чего нужны спецсимволы `^`, `$`, `.`, `|`?**
   - **`^`**: Начало строки.
   - **`$`**: Конец строки.
   - **`.`**: Любой одиночный символ.
   - **`|`**: Логическое "или" (альтернативы).

9. **Когда использовать экранирующий символ `\`?**
   Экранирующий символ используется для того, чтобы трактовать специальные символы как обычные (например, `\.` для точки или `\\` для обратного слэша).

10. **Что такое символьные классы? Как работают символьные классы `\w`, `\W`, `\d`, `\D`, `\s`, `\S`, `\b`, `\B`?**
    - **`\w`**: Любая буква, цифра или нижнее подчеркивание.
    - **`\W`**: Любой не символ из `\w`.
    - **`\d`**: Любая цифра.
    - **`\D`**: Любой нецифровой символ.
    - **`\s`**: Пробельный символ.
    - **`\S`**: Любой непробельный символ.
    - **`\b`**: Граница слова.
    - **`\B`**: Не граница слова.

    **Почему `\babc\b` сработает для строки `abc`, но `\b\.\.\b` не сработает для строки `..`?**
    - `\b` указывает на границу слова, а `..` не имеет границ слов, поэтому `\b\.\.\b` не работает.

11. **Группы**
    - **Что такое?** Группы позволяют захватывать подстроки и применять к ним операции.
    - **Как создать?** Используйте круглые скобки: `(pattern)`.
    - **Когда могут быть полезны?** Для захвата частей строки и повторного использования их в выражении.
    - **Можно ли создавать вложенные группы?** Да, можно создавать группы внутри других групп.
    - **Как сделать незапоминающуюся группу?** Используйте `(?:pattern)`.
    - **Как использовать значение группы в шаблоне регулярного выражения?** Через ссылку на группу, например, `\1`.
    - **Можно ли дать группе имя? Когда это может быть полезно?** Да, используя `(?<name>pattern)`. Это полезно для повышения читаемости и использования именованных групп в коде.
    - **Как использовать значение группы в новой подстроке в методе `replace`?** Используйте `$n` или `${n}` в строке замены.

12. **Как работают опережающие и ретроспективные проверки?**
    - **Опережающие проверки (lookahead):** Проверяют, что за текущей позицией следует определенный шаблон, не включая его в результат. Пример: `\d(?=\D)` (цифра, за которой следует нецифровой символ).
    - **Ретроспективные проверки (lookbehind):** Проверяют, что перед текущей позицией следует определенный шаблон, не включая его в результат. Пример: `(?<=\d)\D` (нецифровой символ, за которым следует цифра).

13. **Решение задач:**
    - **Перевод Function Expression в Function Declaration:**
      ```javascript
      /const\s+(\w+)\s*=\s*function\s*\(([^)]*)\)\s*{([^}]*)}/g
      ```
      Замена на:
      ```javascript
      function $1($2) {$3}
      ```

    - **Перевод Function Declaration в Function Expression:**
      ```javascript
      /function\s+(\w+)\s*\(([^)]*)\)\s*{([^}]*)}/g
      ```
      Замена на:
      ```javascript
      const $1 = function($2) {$3}
      ```

    - **Поиск команд Телеграма в сообщении:**
      ```javascript
      /\/[a-zA-Z0-9_]+/g
      ```
      Это регулярное выражение найдет команды в формате `/command` или `/command123`, включая любые символы после слэша.

### Итераторы и генераторы

1. **Что такое итерируемые объекты и итераторы? Рассказать про протоколы итерирования:**
   - **Iterable:** Объект, который можно перебирать с помощью `for...of` и который имеет метод `Symbol.iterator`.
   - **Iterator:** Объект, который возвращает следующий элемент при каждом вызове метода `next()`.
   - **Async Iterable:** Объект, который можно перебирать асинхронно, имеющий метод `Symbol.asyncIterator`.
   - **Async Iterator:** Объект, возвращающий асинхронные итераторы с методом `next()` возвращающим промис.

2. **Зачем нужны итерируемые объекты, если уже есть массивы?**
   Итерируемые объекты позволяют создавать пользовательские структуры данных и интерфейсы для итерации, не ограничиваясь только массивами.

3. **В чём разница между перебором массива и итерируемого объекта через конструкции: `for`, `for of`, `for in`?**
   - **`for`**

: Итерирует по индексам/ключам, подходяще для массивов и объектов.
   - **`for of`**: Итерирует по значениям и работает с итерируемыми объектами.
   - **`for in`**: Итерирует по ключам объекта, не рекомендуется для массивов.

4. **Что такое генераторы? Где они могут пригодиться?**
   Генераторы — это функции, которые могут приостанавливать своё выполнение и возобновлять его позже с сохранением состояния. Они полезны для ленивой генерации данных, обработки потоков данных и упрощения асинхронного кода.

5. **Рассказать про методы объекта-генератора:**
   - **`next(value)`**: Возвращает следующий результат генератора. Можно передать значение для использования в выражении `yield`.
   - **`throw(exception)`**: Генерирует исключение внутри генератора.
   - **`return(value)`**: Завершает генератор и возвращает значение.

6. **Что такое композиция генераторов?**
   Композиция генераторов — это использование одного генератора внутри другого для создания сложных итераторов.

7. **Почему в качестве ключевого слова используется именно `yield`?**
   Ключевое слово `yield` было выбрано для того, чтобы обозначить точку, где выполнение функции приостанавливается и возвращается управление вызывающему коду.

8. **Как происходит работа с async generators?**
   Async генераторы работают так же, как и синхронные, но их метод `next()` возвращает промис. Они используются для обработки асинхронных потоков данных.

### Set, Map

1. **Set**
   - **Что такое?** Множество уникальных значений.
   - **Как создать инстанс?**
     ```javascript
     const set = new Set();
     ```
   - **Какие методы существуют у инстанса?**
     - **`add(value)`**: Добавляет значение.
     - **`delete(value)`**: Удаляет значение.
     - **`has(value)`**: Проверяет наличие значения.
     - **`clear()`**: Очищает множество.
     - **`size`**: Возвращает количество элементов.
   - **Как можно перебрать?** Используйте `for...of` или методы `forEach`.
   - **Может ли содержать одинаковые элементы?** Нет, содержит только уникальные элементы.
   - **Где может быть полезен?** Для хранения уникальных значений и быстрой проверки наличия.

2. **Map**
   - **Что такое?** Коллекция пар "ключ-значение".
   - **Как создать инстанс?**
     ```javascript
     const map = new Map();
     ```
   - **Какие методы существуют у инстанса?**
     - **`set(key, value)`**: Добавляет или обновляет элемент.
     - **`get(key)`**: Получает значение по ключу.
     - **`delete(key)`**: Удаляет элемент.
     - **`has(key)`**: Проверяет наличие ключа.
     - **`clear()`**: Очищает карту.
     - **`size`**: Возвращает количество элементов.
   - **Как можно перебрать?** Используйте `for...of`, методы `forEach`, или методы `keys()`, `values()`, `entries()`.
   - **В чем отличие от обычного объекта?** Map сохраняет порядок элементов, позволяет любые типы ключей и имеет больше методов для работы с данными.
   - **Где может быть полезен?** Для хранения пар данных с уникальными ключами и быстрой доступностью по ключу.

3. **WeakSet/WeakMap**
   - **Что такое?** Коллекции для слабых ссылок, которые не предотвращают сборку мусора.
   - **В чем отличия от Set/Map?** WeakSet и WeakMap не предотвращают сборку мусора элементов, если на них нет других ссылок.
   - **Где могут быть полезны?** Для ситуаций, когда важно избегать утечек памяти при использовании объектов в качестве ключей или значений.