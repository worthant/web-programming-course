# Микрокр 3

|     | 01234                                                  | 56789                                                                                  |
| --- | ------------------------------------------------------ | -------------------------------------------------------------------------------------- |
| 1.  | В чём отличия в `типизации` между JS и Java?           | Для чего нужна конструкция `@extend` в SCSS?                                           |
| 2.  | Для чего нужна инструкция `@mixin` в SCSS?             | Принципы реализации `наследования` в JS (ES5)                                          |
| 3.  | JS-функция, `удаляющая` все `гиперссылки` со страницы. | JS-функция, `заменяющая` содержимое `адресной строки` браузера на <https://se.ifmo.ru> |

## Варианты `01234`

1. **В чём отличия в `типизации` между JS и Java?**

   |        Java         |                  Javascript                  |
   | :-----------------: | :------------------------------------------: |
   |     статическая     |                 динамическая                 |
   |       сильная       |                    слабая                    |
   | has primitive types | everything - object (except null, undefined) |

   ### Пояснения:

   `Статическая` - тип переменной должен быть объявлен явно и не может быть изменён после объявления (_compile-time checking_)

   ```java
   int x = 10;
   x = "hello";  // Компилятор выдаст ошибку
   ```

   `Динамическая` - тип переменной определяется во время выполнения программы, может меняться (_runtime checking_)

   ```js
   let x = 10;
   x = "hello"; // вообще пахую
   ```

   `Сильная`(аура) - нет автоматического преобразования между большинством типов.

   ```java
   int x = 10;
   double y = x;  // Необходимо явное приведение: double y = (double) x;
   ```

   `Слабая` - автоматическое преобразование типов

   ```js
   let x = 10;
   let y = "10";
   console.log(x + y); // Вывод "1010"
   ```

   `Примитивные типы` - **int**, **char**, **float**, **double**, ...

   - Не объекты, не имеют методов
   - в Js всё объекты, поэтому можно так:

     ```js
     let str = "hello";
     console.log(str.toUpperCase()); // HELLO
     ```

2. **Для чего нужна инструкция `@mixin` в SCSS?**

    - `@mixin` используется для создания многоразовых CSS-блоков кода.
    - Можно вставлять в различные части SCSS-кода с помощью инструкции `@include`
    - Устранение повторения кода, good mantainability, динамические стили
    > миксины могут принимать аргументы, в этом их отличие от `extends`

   ```SCSS
   @mixin circle($radius) {
       width: $radius;
       height: $radius;
   }

   .some-div {
       @include circle(10px);
   }
   ```

3. **JS-функция, `удаляющая` все `гиперссылки` со страницы**:

    ```js
    function removeHyperlinks() {
        const links = document.querySelectorAll('a');
        links.forEach(link => link.remove());
    }
    ```

## Варианты `56789`

1. **Для чего нужна конструкция `@extend` в SCSS?**

    - `@extend` - наследование всех стилей одного селектора в другой.
    - наследование стилий, не требующих динамических значений
    > в отличие от миксинов, это статичные стили (нет передаваемых аргументов)

    ```SCSS
    %message {
        border: 1px solid black;
    }

    .success {
        @extend %message;
        border-color: green;
    }
    ```

    - если использовать `.message` вместо `%`, то этот класс должен существовать в итоговом CSS-файле.
    - При использовании `шаблонов стилей` с `%` - плейсхолдеры не будут компилироваться в итоговый CSS без приписки `@extend`

2. **Принципы реализации `наследования` в JS (ES5):**

    - С помощью прототипов (`prototype`).
    > Каждый объект в Js имеет свой "прототип", от которого он наследует свойства и методы

    - Функция-конструктор для создания объектов
    > В ES5, "классы" создаются с использованием функций-конструкторов.

    - Добавление методов в прототип
    > Методы, которые должны быть унаследованы, добавляются в прототип функции-конструктора.

    - Наследование
    > Прототип дочернего объекта ссылается на экземпляр родительского объекта.

    #### Таким образом, наследование в ES5 реализуется через `прототипы` и `функции-конструкторы`.

3. **JS-функция, `заменяющая` содержимое `адресной строки` браузера на <https://se.ifmo.ru>:**

    ```js
    function changeURL() {
        window.location.href = 'https://se.ifmo.ru';
    }
    ```

## Just some additional questions for prep

1. RESTful request using AJAX

    - using XMLHttpRequest:

    ```js
    var xhr = new XMLHttpRequest();
    xhr.open("GET", "https://api.example.com/data", true);
    xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && xhr.status === 200) {
        console.log(JSON.parse(xhr.responseText));
    }
    };
    xhr.send();
    ```

    - using Fetch API:

    ```js
    fetch('https://api.example.com/data')
        .then(response => response.json())
        .then(data => console.log(data))
        .catch(error => console.error(error));
    ```

    - using Axious:

    ```js
    axios.get('https://api.example.com/data')
        .then(response => {
            console.log(response.data);
        });
    ```

    - jQuery `$.ajax`:

    ```js
    $.ajax({
        url: 'https://api.example.com/data',
        type: 'GET',
        success: (data) => {
            console.log(data);
        },
        error: (error) => {
            console.error(error);
        }
    });
    ```

    - jQuery `$.get`:

    ```js
    $.get("https://api.example.com/data", function(data) {
        console.log(data);
    })  
    .fail(function(err) {
        console.error(err);
    });
    ```

    - SuperAgent:

    ```js
    const superagent = require('superagent');

    superagent.get('https://api.example.com/data')
        .then(response => {
            console.log(response.body);
        })
        .catch(error => {
            console.error(error);
        });
    ```

2. What is hoisting and how does it work in JavaScript?

    - Hoisting is a behavior where variable and function declarations are moved to the top of their containing scope during the compilation phase. Variables declared with var are hoisted and initialized with undefined, while function declarations are hoisted with their full definition.

    ```js
    // This will not throw an error because of hoisting
    console.log(a); // undefined
    var a = 5;
    ```

3. Tricky ECMAScript Topics

    - Block Scope Variables: `let` and `const` have block scope. `var` has function scope.
    - Template Literals: Allows embedding expressions: `const greeting = 'Hello, ${name}!';`
    - Destructuring:
        - Unpacking values from arrays or properties from objects: `const { a, b } = obj;`
        - Value Swapping: `[a, b] = [b, a];`
    - OOP: `class Animal {}`, `class Dog extends Animal {}`
    - Promises and Callbacks:

4. Webcosket

    - protocol that establishes a two-way, full-duplex communication channel over a single TCP connection. It's commonly used in real-time applications like chat, notifications, and live updates.

    ```js
    const socket = new WebSocket('ws://example.com');
    ```

    ```js
    socket.onopen = () => {
        socket.send('Hello, Server!');
    };

    socket.onmessage = (event) => {
        console.log(`Received: ${event.data}`);
    };
    ```

5. DHTML

    - combination of DOM, HTML, CSS, and JavaScript to create dynamic and interactive web pages

    ```html
    <!DOCTYPE html>
    <html>
    <head>
    <title>Simple DHTML Example</title>
    </head>
    <body>

    <h1 id="myHeader">Hello World!</h1>
    <button type="button" onclick="changeText()">Click Me!</button>

    <script src="script.js"></script>

    </body>
    </html>
    ```

