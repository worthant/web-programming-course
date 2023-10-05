# Микрокр 4

|    | 13579                                                                                                                             | 24680                                                                                                                                                |
|----|-----------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1. | Почему `RequestDispatcher` через контекст сервлета можно получить только по абсолютному пути?                                     | В каком порядке вызываются фильтры в `FilterChain`?                                                                                                  |
| 2. | Какие реализации интерфейса `javax.servlet.Servlet` входят в состав `Jakarta EE 9`, и в чём разница между ними?                   | Где лучше хранить содержимое корзины покупателя - в `атрибутах запроса`, `сессии` или `контекста` и почему?                                          |
| 3. | Сервлёт, перенаправляющий запрос странице `index.jsp`, только если в нём присутствует идентификатор пользователя(cookie `userId`) | Конфигурация, формирующая 2 экземпляра сервлета `com.example.MyServlet`, обрабатывающих запросы к ресурсам `/page1.do` и `/page2.do` соответственно. |

## Варианты `13579`

### **1. Почему `RequestDispatcher` через контекст сервлета можно получить только по абсолютному пути?**

> `ServletRequest` предполагает перенаправление на ресурс, находящийся в той же директории, что и текущий сервлёт (или
> JSP).
> - Это удобно для операций внутри одного модуля или компонента веб-приложения
>
> `ServletContext` предполагает перенаправление на любой ресурс в пределах веб-приложения с помощью абсолютного пути.

#### `ServletContext` находится на уровне веб-приложения, он существует в единственном экземпляре на весь жизненный цикл веб-приложения и доступен ВСЕМ сервлётам

#### `ServletRequest` создаётся на уровне **отдельного** клиентского запроса. На каждый запрос создаётся новый объект ServletRequest. Он существует только на время обработки текущего запроса и после ответа клиенту уничтожается.

- поэтому, если пытаться получить `RequestDispatcher` через контекст сервлёта по относительному пути, то может
  возникнуть двусмысленность и неясность.

К примеру:

```java
WebApp
├── WEB-INF
│   └── web.xml
├── admin
│   └── dashboard.jsp
├── user
│   └── profile.jsp
└── index.jsp

// В каком-то AdminServlet.java
RequestDispatcher dispatcher = getServletContext().getRequestDispatcher("dashboard.jsp");

// относительно чего должен быть этот путь? относительно корня? текущего расположения каждого сервлёта?
// поэтому надо так:
RequestDispatcher dispatcher = getServletContext().getRequestDispatcher("/admin/dashboard.jsp");
```

- `Абсолютный путь` нужен, чтобы устранить двусмысленность и явно указать, к какому ресурсу внутри всего веб-приложения
  мы хотим получить доступ.
- Это обеспечивает более строгое и ясное управление ресурсами.

### **2. Какие реализации интерфейса `javax.servlet.Servlet` входят в состав `Jakarta EE 9`, и в чём разница между ними?**

1. `GenericServlet` - базовая протокол-независимая реализация интерфейса `Servlet`. Предоставляет реализацию для всех методов `Servlet`, кроме service()
- **init()** - инициализация сервлёта
- **log()** - из `ServletContext`, метод чтоб записать сообщение в лог файл сервлёта
- **destroy()** - вызывается при завершении работы сервлёта
- **getServletConfig()** - вернёт объект `ServletConfig` (параметры инициализации + контекст сервлёта)
- **getServletInfo()** - вернёт строку с информацией о сервлёте (автор, версия, e.t.c.)
2. `HttpServlet` - наследуется от `GenericServlet`, расширяя его функциональность для обработки HTTP-запросов:
- **doGet(req, res)**
- **doPost(req, res)**
- **doPut(req, res)**
- ...
3. `FacesServlet` - ключевой сервлёт в фреймворке JSF. Обрабатывает все запросы к JSF-страницам, обеспечивая жизненный цикл этого фреймфорка.

#### Summary:
- `GenericServlet` - базовая протокол-независимая реализация `Servlet`, для создания достаточно преопределить `service()`
- `HttpServlet` - его наследник, http-ориентированная реализация сервлёта
- `FacesServlet` - нужен для обработки жизненного цикла `JSF`, скрывая детали реализации. 

### **3. Сервлёт, перенаправляющий запрос странице `index.jsp`, только если в нём присутствует идентификатор пользователя (cookie `userId`)**

```java
import javax.servlet.*;
import javax.servlet.http.*;
import java.io.IOException;

public class MyServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie[] cookies = req.getCookies();
        boolean hasUserId = false;

        if (cookies != null) {
            for (Cookie cookie : cookies) {
                if ("userId".equals(cookie.getName())) {
                    hasUserId = true;
                    break;
                }
            }
        }

        if (hasUserId) {
            RequestDispatcher dispatcher = req.getRequestDispatcher("/index.jsp");
            dispatcher.forward(req, resp);
        } else {
            resp.getWriter().write("User ID not found.");
        }
    }
}

```

## Варианты `24680`

### **1. В каком порядке вызываются фильтры в `FilterChain`?**

- `FilterChain` - буквально, цепочка фильтров. Позволяет последовательно обрабатывать запрос.
- Фильтры в `FilterChain` вызываются в порядке, в котором они были определены в файле конфигурации `web.xml` или `аннотациями`.
- Каждый фильтр выполняет свою задачу и затем передает управление следующему фильтру в цепочке, вызывая метод `doFilter()` из объекта `FilterChain`.

```java
public class FilterOne implements Filter {
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws Exception {
        PrintWriter out = resp.getWriter();
        out.print("FilterOne: Дописываем что-то перед телом ответа");
    
        chain.doFilter(req, resp); // вызываем следующий фильтр в цепочке
    
        out.print("FilterOne: Дописываем что-то после тела ответа");
    }
    // ... другие методы как init, destroy  
}

public class FilterTwo implements Filter {
    public void doFilter(...) {
        PrintWriter out = resp.getWriter();
        out.print("FilterTwo: Дописываем что-то перед телом ответа");
    
        chain.doFilter(req, resp); // вызываем следующий фильтр в цепочке
    
        out.print("FilterTwo: Дописываем что-то после тела ответа");
    }
    ...
}

...
```

### **2. Где лучше хранить содержимое корзины покупателя - в `атрибутах запроса`, `сессии` или `контекста` и почему?**

- ответ: в сессии, а для того, чтобы понять почему, давайте проведём сравнительный анализ:
- - что нужно для корзины покупателя? Чтобы она сохранялась на протяжении взаимодействия юзера с приложением, учитывая вход/выход и прочие ситуации, при которых она должна сохраниться

#### Атрибут запроса

- срок жизни - на протяжении одного запроса. Сл-но, данные будут потеряны сразу же после его завершения
- `Не подходит`

#### Контекст

- срок жизни `ServletContext` ограничен **всем** временем работы веб-приложения (слишком долго)
- контекст сервлёта доступен **для всех пользователей** (зачем другим пользователям знать о вашей корзине?)
- `Не подходит`, т.к. сюда помещаются данные, которые касаются всего приложения, а не каждого юзера

#### Сессия

- доступна на протяжении всей **пользовательской сессии**
- Можно легко проверить, есть ли активная сессия, и соответственно, есть ли корзина, которую нужно отобразить пользователю.
```java
if (session.getAttribute("cart") != null) {
    Cart cart = (Cart) session.getAttribute("cart");
    // Отобразить корзину
}
```
- удобное использование `Cookie-based Session Tracking`
```java 
HttpSession session = request.getSession();
session.setMaxInactiveInterval(15*60);
```
- => `Подходит`. Сессия является наиболее подходящим местом для хранения корзины, поскольку она сохраняет состояние между разными запросами от одного и того же пользователя, в сессии удобнее всего хранить и легко изменять содержимое корзины покупателя на все время сеанса.

### **3. Конфигурация, формирующая 2 экземпляра сервлета `com.example.MyServlet`, обрабатывающих запросы к ресурсам `/page1.do` и `/page2.do` соответственно.**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE web-app>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
<servlet>
    <servlet-name>myServlet1</servlet-name>
    <servlet-class>com.example.MyServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>myServlet1</servlet-name>
    <url-pattern>/page1.do</url-pattern>
</servlet-mapping>

<servlet>
    <servlet-name>myServlet2</servlet-name>
    <servlet-class>com.example.MyServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>myServlet2</servlet-name>
    <url-pattern>/page2.do</url-pattern>
</servlet-mapping>

```
