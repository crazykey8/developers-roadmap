**SOLID** — это пять принципов объектно-ориентированного проектирования, которые помогают создавать гибкие, расширяемые и легко поддерживаемые программы. Принципы особенно важны при проектировании классов и архитектуры.

---

### 🌟 Кратко о каждом принципе:

1. **S** — _Single Responsibility Principle_ (Принцип единственной ответственности)
   → Класс должен иметь только одну причину для изменения (одну зону ответственности).
   ✅ Хорошо делит обязанности между сущностями.

2. **O** — _Open/Closed Principle_ (Принцип открытости/закрытости)
   → Классы должны быть **открыты для расширения**, но **закрыты для изменения**.
   ✅ Новое поведение добавляется без изменения существующего кода.

3. **L** — _Liskov Substitution Principle_ (Принцип подстановки Барбары Лисков)
   → Подклассы должны быть взаимозаменяемы с базовыми классами.
   ✅ Везде, где ожидается базовый тип, можно использовать производный класс без ошибок.

4. **I** — _Interface Segregation Principle_ (Принцип разделения интерфейса)
   → Клиенты не должны зависеть от интерфейсов, которые они не используют.
   ✅ Лучше много специализированных интерфейсов, чем один общий.

5. **D** — _Dependency Inversion Principle_ (Принцип инверсии зависимостей)
   → Модули верхнего уровня не должны зависеть от модулей нижнего уровня — оба должны зависеть от абстракций.
   ✅ Зависимость от интерфейсов, а не от конкретных реализаций.

---

### ✅ Примеры на TypeScript:

#### 1. **S** — Single Responsibility

```ts
// ❌ Плохо: Один класс делает всё
class UserManager {
  createUser() {
    /*...*/
  }
  sendEmail() {
    /*...*/
  }
}

// ✅ Хорошо: Разделение ответственности
class UserService {
  createUser() {
    /*...*/
  }
}

class EmailService {
  sendEmail() {
    /*...*/
  }
}
```

#### 2. **O** — Open/Closed

```ts
// ✅ Можно расширить, не изменяя исходный код
abstract class Shape {
  abstract area(): number;
}

class Circle extends Shape {
  constructor(public radius: number) {
    super();
  }
  area() {
    return Math.PI * this.radius ** 2;
  }
}

class Square extends Shape {
  constructor(public side: number) {
    super();
  }
  area() {
    return this.side ** 2;
  }
}

function totalArea(shapes: Shape[]) {
  return shapes.reduce((sum, shape) => sum + shape.area(), 0);
}
```

#### 3. **L** — Liskov Substitution

```ts
class Bird {
  fly() {
    console.log("Flying");
  }
}

class Duck extends Bird {}

class Ostrich extends Bird {
  fly() {
    throw new Error("Ostrich can't fly"); // ❌ нарушает принцип Лисков
  }
}

// ✅ Правильный подход:
class Animal {}
class FlyingBird extends Animal {
  fly() {
    console.log("Flying");
  }
}

class Duck2 extends FlyingBird {}
class Ostrich2 extends Animal {} // не умеет летать — и не должен
```

#### 4. **I** — Interface Segregation

```ts
// ❌ Плохо: интерфейс содержит лишние методы
interface Worker {
  work(): void;
  eat(): void;
}

class Robot implements Worker {
  work() {
    /*...*/
  }
  eat() {
    throw new Error("Robot doesn't eat");
  } // ❌
}

// ✅ Хорошо: разделённые интерфейсы
interface Workable {
  work(): void;
}

interface Eatable {
  eat(): void;
}

class Human implements Workable, Eatable {
  work() {
    /*...*/
  }
  eat() {
    /*...*/
  }
}

class Robot2 implements Workable {
  work() {
    /*...*/
  }
}
```

#### 5. **D** — Dependency Inversion

```ts
// ❌ Плохо: высокая зависимость от конкретного класса
class MySQLDatabase {
  save(data: string) {
    /*...*/
  }
}

class App {
  db = new MySQLDatabase(); // жесткая связка
  saveData(data: string) {
    this.db.save(data);
  }
}

// ✅ Хорошо: зависимость от абстракции
interface Database {
  save(data: string): void;
}

class MySQL implements Database {
  save(data: string) {
    /*...*/
  }
}

class MongoDB implements Database {
  save(data: string) {
    /*...*/
  }
}

class App2 {
  constructor(private db: Database) {}
  saveData(data: string) {
    this.db.save(data);
  }
}
```

ООП (Объектно-Ориентированное Программирование) — это парадигма программирования, основанная на концепции объектов, которые представляют собой экземпляры классов и объединяют данные (состояние) и методы (поведение).

Отлично! Вот примеры **каждого принципа ООП отдельно** на **TypeScript**, чтобы было нагляднее:

---

### 🧠 1. **Абстракция**

Скрытие сложных деталей реализации, предоставляя простой интерфейс:

```ts
class CoffeeMachine {
  private heatWater(): void {
    console.log("Вода нагрета");
  }

  private grindBeans(): void {
    console.log("Зерна смолоты");
  }

  public makeCoffee(): void {
    this.grindBeans();
    this.heatWater();
    console.log("Кофе готов!");
  }
}

const machine = new CoffeeMachine();
machine.makeCoffee(); // Выводит: Зерна смолоты, Вода нагрета, Кофе готов!
// Внутренние методы недоступны напрямую — это и есть абстракция
```

---

### 🔐 2. **Инкапсуляция**

Ограничение доступа к внутренним свойствам/методам объекта:

```ts
class BankAccount {
  private balance: number;

  constructor(initial: number) {
    this.balance = initial;
  }

  public deposit(amount: number): void {
    if (amount > 0) this.balance += amount;
  }

  public getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount(1000);
account.deposit(500);
console.log(account.getBalance()); // 1500
// account.balance = 99999; ❌ Нельзя: свойство private
```

---

### 🧬 3. **Наследование**

Новый класс наследует свойства и поведение родительского класса:

```ts
class Animal {
  move(): void {
    console.log("Животное движется");
  }
}

class Dog extends Animal {
  bark(): void {
    console.log("Собака лает");
  }
}

const dog = new Dog();
dog.move(); // унаследовано от Animal
dog.bark(); // собственный метод Dog
```

---

### 🔁 4. **Полиморфизм**

Одинаковый метод — разное поведение в разных классах:

```ts
class Shape {
  draw(): void {
    console.log("Рисуется фигура");
  }
}

class Circle extends Shape {
  draw(): void {
    console.log("Рисуется круг");
  }
}

class Square extends Shape {
  draw(): void {
    console.log("Рисуется квадрат");
  }
}

const shapes: Shape[] = [new Circle(), new Square()];

shapes.forEach((shape) => shape.draw());
// Вывод:
// Рисуется круг
// Рисуется квадрат
```

---

Если хочешь — могу визуализировать это в диаграмме или сделать короткий учебный проект, использующий все принципы.
