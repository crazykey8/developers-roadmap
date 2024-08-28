### useEffect

#### Нужно ли указывать функции в виде зависимостей эффектов?

Да, если функция используется внутри `useEffect` и зависит от каких-либо внешних переменных, её нужно включить в массив зависимостей. Это необходимо для предотвращения использования устаревших значений переменных в функции. Однако если функция не зависит от переменных из замыкания, её можно не включать в зависимости.

#### Что выведется в консоли для следующего кода, если быстро нажать 5 раз на кнопку, и почему?

```javascript
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    setTimeout(() => {
      console.log(`You clicked ${count} times`);
    }, 3000);
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

В консоли выведется 5 раз текущее значение счётчика на момент срабатывания `setTimeout`. Если нажать на кнопку 5 раз, пока не прошли 3 секунды, то вывод в консоль будет происходить с разными значениями `count`, так как каждый вызов `useEffect` будет запущен с текущим состоянием на момент рендеринга. Например, может быть выведено: `You clicked 0 times`, `You clicked 1 times`, и так далее.

#### Как сделать так, чтобы все 5 раз вывелось последнее значение, то есть цифра 5?

Чтобы все 5 раз выводилось последнее значение, необходимо использовать актуальное состояние `count` на момент срабатывания `setTimeout`. Это можно сделать, используя замыкание:

```javascript
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const timeoutId = setTimeout(() => {
      console.log(`You clicked ${count} times`);
    }, 3000);
    return () => clearTimeout(timeoutId);
  }, [count]);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```

Теперь `useEffect` будет зависеть от `count`, и при каждом его изменении старый таймаут будет очищаться, а новый — запускаться с актуальным значением.

#### Что выведется в консоли для следующего кода, если несколько раз быстро нажать на кнопку?

```javascript
function Example() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log(count);
    if (count % 2 === 0) {
      return () => setTimeout(() => console.log("It was even render"), 1000);
    } else {
      return () => setTimeout(() => console.log("It was odd render"), 1000);
    }
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

В консоли будет сначала выведено текущее значение `count` при каждом рендере. Затем, через 1 секунду после каждого рендера, будут выводиться строки `"It was even render"` или `"It was odd render"`, в зависимости от того, было ли значение `count` четным или нечетным на момент рендеринга.

Если быстро нажать на кнопку несколько раз, то в консоли сначала появится несколько значений `count` (0, 1, 2, 3 и т. д.), а затем (с небольшой задержкой) начнут выводиться соответствующие строки. Поскольку таймауты не очищаются, все строки будут выведены, даже если значение `count` изменилось.

### Performance

#### Для чего в хук `useState` можно передавать функцию вместо `initialState`?

Функцию можно передавать, чтобы избежать повторных вычислений значения начального состояния при каждом рендере. Функция будет вызвана только один раз для инициализации состояния, что улучшает производительность, особенно если начальное значение требует сложных вычислений.

#### Когда передача инлайн-коллбека ухудшает производительность и почему?

Передача инлайн-коллбека может ухудшить производительность, если такой коллбэк используется как пропс для дочернего компонента, который оптимизирован с помощью `React.memo` или аналогичных механизмов. В этом случае при каждом рендере родителя инлайн-коллбек будет считаться новым значением, что приведет к перерендерам дочернего компонента, даже если остальные пропсы не изменились.

#### Как и для чего использовать хуки `useMemo` и `useCallback`?

- **`useMemo`**: используется для мемоизации значений, чтобы повторно использовать результаты дорогостоящих вычислений, если их зависимости не изменились.
- **`useCallback`**: используется для мемоизации функций, чтобы сохранять один и тот же экземпляр функции, если её зависимости не изменились.

Они используются для оптимизации производительности компонентов и предотвращения ненужных рендеров.

**Не всегда нужно оборачивать код в `useMemo`/`useCallback`,** так как сами эти хуки имеют накладные расходы на память и вычисления. Использовать их следует только тогда, когда есть реальная необходимость в оптимизации.

#### Что такое `React.memo`? Для чего он нужен?

`React.memo` — это HOC (Higher-Order Component), который предотвращает повторный рендер компонента, если его пропсы не изменились. Он полезен для оптимизации компонентов, которые часто рендерятся с одинаковыми пропсами.

- **Стоит использовать,** когда компонент принимает примитивные типы данных в качестве пропсов и вычисления в компоненте дорогие.
- **Не стоит использовать,** если компонент рендерится редко или пропсы включают функции или объекты, которые создаются заново при каждом рендере.

**Ограничения на пропсы**: Для правильной работы `React.memo` пропсы должны быть примитивными типами или стабилизированными функциями и объектами (с использованием `useCallback` или `useMemo`).

#### Почему использование `React.memo` для `MemoComponent` таким образом не приносит никакой пользы?

```javascript
const outerComponent = ({ options, dataForClick, onComponentClick }) => {
  const makeClickHandler = (dataForClick) => {
    return () => onComponentClick(dataForClick);
  }

  const enhanceOptions = () => {
    const uppercased = {};
    Object.keys(options).forEach(key => {
      uppercased[key] = options[key].toUpperCase();
    })
    return uppercased;
  }

  return (
    <MemoComponent
      onClick={makeClickHandler(dataForClick)}
      options={enhanceOptions()}
    >
      <div>This is <b>Memo Component!</b></div>
    </MemoComponent>
  );
};
```

В данном примере `MemoComponent` всегда будет перерендериваться, потому что:

1. **`onClick={makeClickHandler(dataForClick)}`**: Функция `makeClickHandler` создаёт новую функцию при каждом рендере, даже если `dataForClick` не изменился. Это приведет к тому, что `MemoComponent` будет получать новый проп `onClick` каждый раз.
   
2. **`options={enhanceOptions()}`**: Функция `enhanceOptions` также создаёт новый объект при каждом рендере, что приводит к изменению пропа `options`, несмотря на то, что входные данные могут оставаться прежними.

Чтобы оптимизировать код, нужно мемоизировать функции и объекты:

```javascript
const outerComponent = ({ options, dataForClick, onComponentClick }) => {
  const makeClickHandler = useCallback(() => onComponentClick(dataForClick), [onComponentClick, dataForClick]);

  const enhanceOptions = useMemo(() => {
    const uppercased = {};
    Object.keys(options).forEach(key => {
      uppercased[key] = options[key].toUpperCase();
    });
    return uppercased;
  }, [options]);

  return (
    <MemoComponent
      onClick={makeClickHandler}
      options={enhanceOptions}
    >
      <div>This is <b>Memo Component!</b></div>
    </MemoComponent>
  );
};
```

Теперь `MemoComponent` будет рендериться только при изменении `dataForClick`, `onComponentClick`, или `options`.