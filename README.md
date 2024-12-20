# **Подробное изучение JavaScript**

## **1. Основы JavaScript**

### **Что такое типы данных? Перечисли основные типы.**

Типы данных в JavaScript делятся на два вида:

- **Примитивные типы данных:**
  - `string`: текстовые данные.
  - `number`: числовые значения (включая `NaN` и бесконечность).
  - `bigint`: числа, превышающие безопасные пределы `number`.
  - `boolean`: логические значения (`true`/`false`).
  - `undefined`: переменная объявлена, но не имеет значения.
  - `null`: специальное значение, указывающее на пустое или несуществующее значение.
  - `symbol`: уникальные идентификаторы.

- **Объекты:**
  - Это сложные структуры данных, которые могут содержать примитивные типы, другие объекты, функции и т. д.

#### **Пример:**
```javascript
// Примитивные типы
let name = "Alice"; // string
let age = 25;       // number
let bigNum = 123456789123456789n; // bigint
let isStudent = true; // boolean
let notDefined;       // undefined
let emptyValue = null; // null
let id = Symbol("uniqueId"); // symbol

// Объект
let user = {
  name: "Alice",
  age: 25,
  isStudent: true,
};
```

### **Чем отличается `null` от `undefined`?**

- **`undefined`:** указывает, что переменной не присвоено значение.
- **`null`:** указывает на намеренное отсутствие значения.

#### **Пример:**
```javascript
let a; // Переменная объявлена, но ей не присвоено значение
console.log(a); // undefined

let b = null; // Переменной явно присвоено значение "ничего"
console.log(b); // null
```

#### **Различие при проверке:**
```javascript
console.log(typeof a); // "undefined"
console.log(typeof b); // "object" (особенность языка)
```

### **Что такое NaN и как проверить его?**

- `NaN` (Not-a-Number) — это специальное значение, которое указывает, что операция не смогла вернуть число.

#### **Пример:**
```javascript
let result = "text" * 2; // Некорректная операция
console.log(result); // NaN
```

#### **Как проверить:**
```javascript
console.log(Number.isNaN(result)); // true — точная проверка
console.log(isNaN("text"));        // true — менее строгая проверка
```

### **Как работает `==` и `===`?**

- `==` (нестрогое сравнение): приводит значения к одному типу перед сравнением.
- `===` (строгое сравнение): сравнивает без приведения типов.

#### **Пример:**
```javascript
console.log(5 == "5");  // true — строка "5" преобразуется в число
console.log(5 === "5"); // false — разные типы
```

### **Какое значение вернет `[] == ![]`?**

- Оператор `![]` возвращает `false`, так как массив считается истинным (`truthy`).
- Сравнение `[] == false` преобразует массив в число (`0`) и сравнивает его с `false` (тоже преобразуется в `0`).

#### **Итог:**
```javascript
console.log([] == ![]); // true
```

## **2. Функции**

### **Что такое функция? Как ее объявить?**

- Функция — это блок кода, который выполняет определенную задачу.

#### **Пример:**
```javascript
function greet(name) {
  return `Hello, ${name}!`; // Возвращает строку с приветствием
}
console.log(greet("Alice")); // Hello, Alice!
```

### **В чем разница между `function declaration` и `expression`?**

- **Function Declaration:**
  - Имеет имя.
  - Можно вызывать до объявления.
- **Function Expression:**
  - Присваивается переменной.
  - Нельзя вызывать до объявления.

#### **Пример:**
```javascript
// Function Declaration
function add(a, b) {
  return a + b;
}

// Function Expression
const subtract = function(a, b) {
  return a - b;
};

// Вызов
console.log(add(5, 3));      // 8
console.log(subtract(5, 3)); // 2
```

### **Что такое стрелочные функции?**

- Стрелочные функции имеют сокращенный синтаксис и не создают собственного `this`.

#### **Пример:**
```javascript
const multiply = (a, b) => a * b; // Сокращенный синтаксис
console.log(multiply(2, 3)); // 6
```

### **Что такое замыкание?**

- Замыкание — это функция, которая «запоминает» переменные из внешнего контекста.

#### **Пример:**
```javascript
function createCounter() {
  let count = 0;
  return function() { // Замкнутая функция
    count++;
    return count;
  };
}

const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

### **Как работает `this`?**

- `this` зависит от контекста вызова:
  - В глобальной области `this` — это `window` (или `undefined` в strict mode).
  - В методах объекта `this` ссылается на объект.

#### **Пример:**
```javascript
const obj = {
  name: "Alice",
  greet() {
    console.log(this.name); // Ссылается на obj
  },
};
obj.greet(); // Alice
```

## **3. Асинхронность**

### **Что такое Promise?**

- `Promise` представляет результат асинхронной операции.

#### **Пример:**
```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => resolve("Done!"), 1000); // Асинхронная операция
});

promise.then(console.log); // Done!
```

### **Как использовать `async/await`?**

- `async/await` упрощает работу с асинхронным кодом.

#### **Пример:**
```javascript
async function fetchData() {
  const response = await fetch("https://api.example.com");
  const data = await response.json();
  console.log(data);
}
```

### **Что такое Event Loop?**

- `Event Loop` обеспечивает выполнение кода и управление асинхронными операциями.

#### **Пример:**
```javascript
console.log("Start");

setTimeout(() => console.log("Timeout"), 0); // Асинхронный код
console.log("End");

// Порядок вывода:
// Start
// End
// Timeout
```


### **Разница между `var`, `let`, и `const`**

- **`var`**:
  - Область видимости: функциональная (не блочная).
  - Может быть переобъявлена и перезаписана.
  - Подвержена всплытию (hoisting), но значение `undefined` до инициализации.

- **`let`**:
  - Область видимости: блочная.
  - Нельзя переобъявить в одной области видимости.
  - Подвержена всплытию, но нельзя использовать до инициализации (ReferenceError).

- **`const`**:
  - Область видимости: блочная.
  - Нельзя переобъявить и перезаписать.
  - Используется для значений, которые не изменяются.

#### **Пример**
```javascript
function example() {
  var x = 10;
  if (true) {
    var x = 20; // То же имя, перезаписывается
    let y = 30; // Локальная область видимости
    const z = 40; // Константа
  }
  console.log(x); // 20
  // console.log(y); // ReferenceError: y is not defined
  // console.log(z); // ReferenceError: z is not defined
}
example();
```

---

### **Что такое всплытие (hoisting)?**

Всплытие (hoisting) — это механизм JavaScript, при котором объявления переменных и функций перемещаются в начало своей области видимости во время выполнения. Это позволяет использовать переменные и функции до их явного объявления в коде. Однако переменные, объявленные с помощью `let` и `const`, также всплывают, но недоступны до инициализации (временная мёртвая зона).

#### **Пример**
```javascript
// Переменная, объявленная с помощью var
console.log(a); // undefined (всплытие переменной)
var a = 10;

// Переменная, объявленная с помощью let
// console.log(b); // ReferenceError: b is not defined
let b = 20;

// Функция, объявленная с помощью function declaration
hoistedFunction(); // "I am hoisted!"
function hoistedFunction() {
  console.log("I am hoisted!");
}

// Функция, объявленная с помощью function expression
// nonHoistedFunction(); // TypeError: nonHoistedFunction is not a function
const nonHoistedFunction = function() {
  console.log("I am not hoisted!");
};
```

---

### **Что такое строгий режим (`"use strict"`)?**

Строгий режим (`"use strict"`) — это режим выполнения JavaScript, который делает код более безопасным. Он помогает находить ошибки на ранних стадиях, запрещает некоторые небезопасные действия и улучшает производительность за счёт оптимизации компиляции.

**Особенности:**
- Запрещает использование необъявленных переменных.
- Запрещает дублирование параметров функции.
- Отключает автоматическое приведение контекста к глобальному объекту.

#### **Пример**
```javascript
"use strict";

x = 10; // Ошибка: x не объявлен
function example(a, a) {
  // Ошибка: дублирование параметров функции
  console.log(a);
}
```

---

### **Что такое замыкание (closure)?**

Замыкание — это функция, которая запоминает область видимости, в которой она была создана, даже после того как эта область завершила выполнение.

#### **Пример**
```javascript
function makeCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}

const counter = makeCounter();
console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

**Объяснение:**
Функция, возвращаемая `makeCounter`, запоминает переменную `count`, даже когда вызов `makeCounter` завершён.

---

### **Что такое прототипное наследование?**

Прототипное наследование позволяет объекту наследовать свойства и методы другого объекта. Каждый объект имеет скрытую ссылку на прототип, который можно получить через `Object.getPrototypeOf(obj)` или `obj.__proto__`.

#### **Пример**
```javascript
const animal = {
  eats: true,
  walk() {
    console.log("Animal walks");
  }
};

const rabbit = {
  jumps: true
};

rabbit.__proto__ = animal;

console.log(rabbit.eats); // true
rabbit.walk(); // "Animal walks"
```

**Объяснение:**
Объект `rabbit` наследует свойства и методы объекта `animal` через цепочку прототипов.

---

### **Что такое промисы и как они работают?**

Промисы — это объект, представляющий результат асинхронной операции, которая может завершиться успехом (`resolved`) или неудачей (`rejected`).

#### **Пример**
```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success!");
  }, 1000);
});

promise
  .then(result => {
    console.log(result); // "Success!"
  })
  .catch(error => {
    console.error(error);
  });
```

**Объяснение:**
- `resolve` — вызывает успешное завершение.
- `reject` — вызывает ошибку.
- `then` обрабатывает успешный результат.
- `catch` обрабатывает ошибки.

