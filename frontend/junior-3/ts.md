### TypeScript

* **Что такое перегрузка функции? Какие есть способы её реализовать?**
  * **Перегрузка функции** — это возможность определять несколько версий одной функции с разными параметрами и типами возвращаемого значения. TypeScript позволяет реализовать перегрузку функции с помощью нескольких объявлений функции с разными сигнатурами.
  * **Способы реализации перегрузки функции:**
    1. **Объявление нескольких сигнатур функции:** 
       ```typescript
       function greet(person: string): string;
       function greet(person: string, age: number): string;
       function greet(person: string, age?: number): string {
         return age ? `Hello, ${person}. You are ${age} years old.` : `Hello, ${person}.`;
       }
       ```
    2. **Использование интерфейсов для перегрузки:** 
       ```typescript
       interface Greet {
         (person: string): string;
         (person: string, age: number): string;
       }
       
       const greet: Greet = (person: string, age?: number): string => {
         return age ? `Hello, ${person}. You are ${age} years old.` : `Hello, ${person}.`;
       };
       ```

  * **Можно ли затипизировать перегрузку конструктора для класса?**
    * Да, можно. В TypeScript перегрузку конструктора можно реализовать следующим образом:
      ```typescript
      class Person {
        name: string;
        age: number;

        constructor(name: string);
        constructor(name: string, age: number);
        constructor(name: string, age?: number) {
          this.name = name;
          this.age = age ?? 0;
        }
      }
      ```

* **Возможно ли в TypeScript объявление рекурсивных type Aliases?**
  * Да, в TypeScript можно объявлять рекурсивные type Aliases. Например:
    ```typescript
    type TreeNode<T> = {
      value: T;
      children?: TreeNode<T>[];
    };
    ```

* **Возможно ли в TypeScript экспортировать в едином неймспейсе и тип, и терм с одинаковым именем?**
  * Нет, это невозможно. В TypeScript нельзя экспортировать тип и значение с одинаковым именем из одного пространства имён или модуля.

* **Что такое const assertion?**
  * **Для чего оно нужно?**
    * Const assertion позволяет TypeScript трактовать значения как константы, предотвращая их изменение. Это позволяет более строго типизировать данные, которые не должны изменяться.
  * **Какие имеются ограничения?**
    * Const assertion работает только для литералов. Это означает, что он не может применяться к значениям, полученным в результате вычислений.
  * **Имеется следующий код:**
    ```typescript
    const names = ['Mary', 'David'];
    const group = {
      participants: names,
      ageRestriction: 18,
    } as const;

    group.participants.push('Henry');
    group.ageRestriction = 12;
    ```
    * **Какая из последних двух строчек вызовет ошибку, а какая нет? Почему?**
      * `group.participants.push('Henry');` вызовет ошибку, потому что `participants` был проаннотирован как `readonly` массив (из-за `as const`), и изменение его содержимого не допускается.
      * `group.ageRestriction = 12;` не вызовет ошибку, потому что свойство `ageRestriction` не было помечено как `readonly`, и его можно изменить.

* **Как создать новый тип на основе имеющегося, с добавлением новых свойств?**
  * **Практическое задание:** Создание типа `SurvivedAvengers<T>`:
    ```typescript
    type Avenger = 'Thor' | 'Hawkeye' | 'Captain America' | 'Iron Man' | 'Dr. Strange' | 'Nick Fury';
    type Head = 'Nick Fury';
    type Dead = 'Iron Man';

    interface IPersonalInformation {
      age: number;
      name: string;
      superpower: any;
    }

    type SurvivedAvengers<T extends Avenger> = {
      [K in Exclude<T, Dead> | Head]: IPersonalInformation;
    };

    // Пример использования
    const personalInformation: IPersonalInformation = {
      age: 25,
      name: 'NameOfAvenger',
      superpower: 'SuperpowerOfAvenger'
    };
    const survivedAvengers: SurvivedAvengers<'Thor' | 'Hawkeye' | 'Iron Man'> = { 
      Thor: personalInformation, 
      Hawkeye: personalInformation, 
      'Nick Fury': personalInformation,
    };
    ```

  * **Теперь сделайте так, чтобы в объекте `survivedAvengers` ключи типа `Head` были опциональными:**
    ```typescript
    type SurvivedAvengersWithOptionalHead<T extends Avenger> = {
      [K in Exclude<T, Dead> | Head]?: IPersonalInformation;
    };
    ```

  * **Теперь сделайте так, чтобы в объекте `survivedAvengers` ключи типа `Head` были опциональными, за исключением ключей, переданных в дженерик:**
    ```typescript
    type SurvivedAvengersWithConditionalHead<T extends Avenger> = {
      [K in Exclude<T, Dead>]: IPersonalInformation;
    } & {
      [K in Head]?: IPersonalInformation;
    };
    ```

* **Каково назначение нижеперечисленных типов?**
  * **`Partial<T>`**
    * Делает все свойства типа `T` необязательными.
  * **`Readonly<T>`**
    * Делает все свойства типа `T` доступными только для чтения.
  * **`Required<T>`**
    * Делает все свойства типа `T` обязательными.
  * **`Record<T, U>`**
    * Создает объектный тип, где ключи типа `T`, а значения типа `U`.
  * **`Pick<T, U>`**
    * Создает тип, состоящий из подмножества свойств типа `T`, указанных в `U`.
  * **`Omit<T, K>`**
    * Создает тип, исключая свойства типа `T`, указанные в `K`.
  * **`Exclude<T, U>`**
    * Создает тип, исключая из `T` типы, указанные в `U`.
  * **`Extract<T, U>`**
    * Создает тип, состоящий из типов `T`, которые также находятся в `U`.
  * **`NonNullable<T>`**
    * Исключает `null` и `undefined` из типа `T`.
  * **`ReturnType<T>`**
    * Извлекает тип возвращаемого значения функции `T`.
  * **`Parameters<T>`**
    * Извлекает типы параметров функции `T`.
  * **`InstanceType<T>`**
    * Извлекает тип экземпляра конструктора `T`.
  * **`ThisType<T>`**
    * Используется для определения типа `this` в объектных литералах.

* **Для чего предназначены Conditional Types?**
  * **Для чего предназначены?**
    * Conditional Types позволяют создавать типы на основе условного выражения, проверяя, соответствует ли тип определенному условию.
  * **Как проявляется дистрибутивность в Conditional Types?**
    * Conditional Types дистрибутивны по отношению к union type, что означает, что условие применяется к каждому элементу union type.
  * **Как удалить составной тип из union type с помощью Conditional Types?**
    * Используя `Exclude<T, U>`, можно удалить типы, которые присутствуют в `U`, из `T`.
    * Пример: `Exclude<'a' | 'b' | 'c', 'a' | 'b'>` возвращает `'c'`.
  * **Допускается ли использовать Conditional Types совместно с Mapped Types?**
    * Да, это допускается. Можно комбинировать Conditional Types с Mapped Types для более сложной типизации.
  * **Для чего нужен `infer`? Допускается ли использовать `infer` для типов не являющихся Conditional Types?**
    * `infer` используется в Conditional Types для извлечения типа из условного выражения.
    * `infer` не может использоваться вне контекста Conditional Types.