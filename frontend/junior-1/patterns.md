Отлично, вот **вопросы и ответы по основам архитектуры и паттернам проектирования**, которые часто встречаются на собеседованиях. Я написал кратко, чётко и с примерами — идеально для подготовки или «шпаргалки».

---

## 📐 **Основы архитектуры и паттерны**

---

### 1. **Что такое архитектура ПО?**

**Ответ:**
Архитектура — это **структура** приложения: как организованы модули, классы, связи между ними, потоки данных и зависимости. Она определяет, **как приложение устроено внутри** и **как его легче поддерживать, масштабировать и тестировать**.

---

### 2. **Что такое архитектурный паттерн?**

**Ответ:**
Архитектурный паттерн — это **готовое решение** для организации кода на уровне всего приложения.
🔸 Примеры: MVC, MVVM, Clean Architecture, Hexagonal Architecture.
Он не про детали реализации, а про **структуру и ответственность компонентов**.

---

### 3. **Что такое шаблон (паттерн) проектирования?**

**Ответ:**
Паттерн проектирования — это **повторяемое решение часто встречающейся задачи** в проектировании кода на уровне классов и объектов.
🔸 Пример: Singleton, Factory, Observer, Strategy и др.
Он помогает **структурировать логику, повысить переиспользуемость и снизить связанность компонентов**.

---

## 🎨 Часто используемые паттерны с примерами:

---

### 4. **Паттерн Singleton**

**Задача:** нужен один единственный экземпляр класса (например, логгер или база данных).

```ts
class Logger {
  private static instance: Logger;

  private constructor() {}

  static getInstance(): Logger {
    if (!Logger.instance) {
      Logger.instance = new Logger();
    }
    return Logger.instance;
  }

  log(msg: string) {
    console.log(msg);
  }
}

const logger1 = Logger.getInstance();
const logger2 = Logger.getInstance();
console.log(logger1 === logger2); // true
```

---

### 5. **Паттерн Factory**

**Задача:** создание объектов без указания точного класса.

```ts
interface Button {
  render(): void;
}

class WindowsButton implements Button {
  render() {
    console.log("Windows Button");
  }
}

class MacButton implements Button {
  render() {
    console.log("Mac Button");
  }
}

class ButtonFactory {
  static createButton(os: string): Button {
    if (os === "Windows") return new WindowsButton();
    return new MacButton();
  }
}

const btn = ButtonFactory.createButton("Mac");
btn.render(); // Mac Button
```

---

### 6. **Паттерн Observer (Наблюдатель)**

**Задача:** подписчики автоматически уведомляются об изменениях.

```ts
class Subject {
  private observers: (() => void)[] = [];

  subscribe(fn: () => void) {
    this.observers.push(fn);
  }

  notify() {
    this.observers.forEach((fn) => fn());
  }
}

const subject = new Subject();
subject.subscribe(() => console.log("Observer 1"));
subject.subscribe(() => console.log("Observer 2"));

subject.notify();
// Observer 1
// Observer 2
```

---

### 7. **Паттерн Strategy**

**Задача:** изменение поведения объекта без изменения его класса.

```ts
interface PaymentStrategy {
  pay(amount: number): void;
}

class PayPal implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paying ${amount} via PayPal`);
  }
}

class CreditCard implements PaymentStrategy {
  pay(amount: number) {
    console.log(`Paying ${amount} with credit card`);
  }
}

class PaymentContext {
  constructor(private strategy: PaymentStrategy) {}

  executePayment(amount: number) {
    this.strategy.pay(amount);
  }
}

const context = new PaymentContext(new PayPal());
context.executePayment(100); // Paying 100 via PayPal
```

---

### 8. **Что такое MVC?**

**Ответ:**
**MVC (Model–View–Controller)** — архитектурный паттерн, разделяющий приложение на:

- **Model** — логика данных, доступ к БД, бизнес-логика.
- **View** — отображение данных пользователю (UI).
- **Controller** — принимает действия пользователя и обновляет модель/представление.

🔸 Применяется во фронтенде, бэкенде, мобильных приложениях.

---

### 9. **Разница между архитектурой и паттерном проектирования?**

| Критерий   | Архитектура                   | Паттерн проектирования          |
| ---------- | ----------------------------- | ------------------------------- |
| Уровень    | Высокий (всего приложения)    | Средний/низкий (структура кода) |
| Назначение | Структура приложения          | Решение типовых задач           |
| Примеры    | MVC, MVVM, Clean Architecture | Singleton, Factory, Observer    |

---

### 10. **Зачем вообще нужны паттерны?**

**Ответ:**
Паттерны помогают:

- писать **понятный и поддерживаемый код**,
- **избегать дублирования логики**,
- **разделять ответственность** между компонентами,
- **улучшать масштабируемость**,
- **упрощать тестирование и модульность**.

---

Хочешь — могу:

- сделать **таблицу всех паттернов с задачами и примерами**;
- или **сделать карточки для тренировки (вопрос — ответ)**.

Скажи, как удобнее тебе учить — я подстроюсь 👇
