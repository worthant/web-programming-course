# Микрокр 2

|![изображение](https://github.com/worthant/web-programming-course/assets/43885024/09302c26-efb3-47b0-961c-d387ae3d4b64)|
|-|


## Варианты `02468`

1. **Отличия протокола `HTTP` от `HTTPS`**:  
   - HTTP не шифрован, HTTPS шифрован (SSL/TLS).
   - HTTPS требует сертификат от центра сертификации.
   - HTTPS обычно работает на порту 443, HTTP — на порту 80.

2. **В каких типах приложений логично использовать `RPC`**:  
   - Микросервисные архитектуры для быстрой коммуникации между сервисами.
   - Распределенные системы для управления ресурсами.
   - Внутренние сетевые сервисы для обмена данными в закрытом окружении.

   **Пояснение**: `RPC (Remote Procedure Call)` — это протокол, позволяющий одной программе выполнять процедуры (код) в другой программе, находящейся в другой физической машине.

3. **Код, к которому применится CSS-селектор (a + b > p)**

```css
a + b > p {
    color: blue;
}
```

```html
<a href="#">Link</a>
<b>
  <p>This will be blue.</p>
</b>

<b>
  <p>This won't be blue.</p>
</b>
<a href="#">Another Link</a>
```

<h4 align="center">Для справки</h4>

> :heavy_plus_sign: выбирает элемент, который следует `непосредственно после` указанного  
> `>` выбирает дочерние элементы у заданного родителя (не все, `вложенность = 1`)

## Варианты `13579`

1. **Отличия метода `POST` от метода `PUT`**
   - POST `добавляет` новую запись или объект на сервер, PUT — `обновляет` существующую.
   - PUT `идемпотентен`, POST — нет.
   - PUT требует полный `URI` ресурса, POST — нет.

<h4 align="center">Для справки</h4>

> `идемпотентный метод` - метод, повторный вызов которого *не меняет результат*
> Заметка про `POST` и `PUT`: например, может получиться так, что посредством POST добавится несколько одинаковых записей, в то время как PUT просто обновит данные до определенного состояния, не вызывая дополнительных эффектов.

2. **Разница между селекторами a#id и a.id**  
   - `a#id` выбирает тег `<a>` с идентификатором `#id`.
   - `a.id` выбирает тег `<a>` с классом `.id`.  

> Селекторы по ID обычно используются для уникальных элементов, селекторы по классу — для групп элементов.

3. **Код для отправки массива почт и паролей с формы**
> 2  варианта реализации, мы хз какой верный и есть ли тут вообще верный

#### 1 вариант (кривоватый, но писать меньше):

```html
<form action="http://www.googol.com/secure" method="GET">
    <label for="email1">Email 1:</label>
    <input type="text" name="emails[]" id="email1"><br>

    <label for="password1">Password 1:</label>
    <input type="password" name="passwords[]" id="password1"><br>

    <!-- Для второго адреса электронной почты -->
    <label for="email2">Email 2:</label>
    <input type="text" name="emails[]" id="email2"><br>

    <label for="password2">Password 2:</label>
    <input type="password" name="passwords[]" id="password2"><br>

    <input type="submit" value="Submit">
</form>
```

#### 2 вариант (писанины больше в разы, зато логика seems to be correct):

```html
<form id="myForm" action="http://www.example.com/secure" method="GET">
    <label for="email">Email:</label>
    <input type="text" id="email" name="currentEmail"><br>
    
    <label for="password">Password:</label>
    <input type="password" id="password" name="currentPassword"><br>
    
    <input type="submit" value="Submit">
</form>

<script>
    // Имеющиеся массивы email и паролей
    let emails = ['email1@example.com', 'email2@example.com'];
    let passwords = ['pass1', 'pass2'];

    // Перехват события submit
    document.getElementById('myForm').addEventListener('submit', function(event) {
        event.preventDefault();  // Предотвратить стандартное поведение формы

        // Добавляем текущие значения в массивы
        const email = document.getElementById('email').value;
        const password = document.getElementById('password').value;

        emails.push(email);
        passwords.push(password);

        // Формируем новый URL для GET-запроса
        const emailString = emails.join(',');
        const passwordString = passwords.join(',');

        const newActionURL = `${this.action}?emails=${emailString}&passwords=${passwordString}`;
        
        // Отправляем запрос
        window.location.href = newActionURL;
    });
</script>
```

Если будет также 5 минут, пишите 1ый варик, если больше - попробуйте написать второй
