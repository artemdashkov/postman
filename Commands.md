# Содержание
- [Теория](#Теория)
- [Методы](#Методы)
- [SOAP API](#SOAP-API)
- [Вкладки Postman](#Вкладки-Postman)
- [Environment](#Environment)
- [Run Collection](#Run-Collection)
- [Синтаксис JS](#Синтаксис-JS)
- [Snippets](#Snippets)

# Теория
- базовый url
- endpoint - функция, которая обрабатывает запрос

# Методы
- GET - retrieves data from an API.
- POST - sends new data to an API. отправляет информацию на сервер, например картинку.
- PUT - replace existing data.
- DELETE - delete existing data.
- PATCH - update some existing data fields

# SOAP API


# Вкладки Postman
- Params - можно указывать ключи-значения, чаще всего используется в методе GET
- Authorization - для того, чтобы куда-то войти получить какие-то права и работать с ресурсом. Существуют разные типы авторизации, например 
    - Basic Auth - есть логин-пароль и мы их передаем в открытом виде, после чего получаем определенные права: администратора, модератора, пользователя. 
    - "OAuth 1.0 и OAuth 2.0" - когда хотим авторизоваться через какую-то соцсеть, например через гугл. 
    - "Bearer Token" - отправляем какую-либо информацию о себе, в ответ получаем токен и потом можем пользоваться токеном для входа в систему.
    - API Key - используется для того, чтобы делать интеграции с разными программами, например если мы хотим внедрить Jira в Testrail
- Headers - служебная информация о том, что мы конкретно отправляем серверу и как сервер себя ведет
- Body - используется только для POST, PUT (в GET не используется), то что мы хотим загрузить на наш сервер
- Pre-request Script - используется для того, чтобы сделать что-то до основного запроса и сюда можно написать какой-то код. 
- Tests - тесты происходят после того, как мы получаем ответ. Например мы хотим проверить что в ответе есть статус код 200. 

- APIs - используется для создания документации внутри Postman

# Environment
is a set of variables that you can reuse in your requests and share with your team. You can reference them in request authentication, query and path parameters, request bodies, and scripts.
набор переменных, которые мы используем для того или иного окружения. как правило команды работают в разных средах, например:
- dev - окружение, которым могут пользоваться разработчики
- stage - стабильная версия, после того как будут пофикшены все найденные баги окружение будет перемещено в prod
- prod - там где работает наш заказчик

### Области действий переменных:
- `Global` - созданные переменные "Globals" будут применяться абсолютно ко всему проекту
- `Collection` - 
- `Environment` - 
- `Data` - 
- `Local` - 

- Variable 
    - INITIAL VALUE - может быть расшарена с другими пользователями и храниться на серверах Postman. Не рекомендуется сохранять чувствительную информацию.
    - CURRENT VALUE - доступна только локально

# Run Collection
- `Persist responses for a session` - необходимо устанавливать чек, чтобы видеть 

# Синтаксис JS
- `var jsonData` или `let jsonData` - объявление переменной. у переменных `var` и `let` разная область видимости. у `var` функциональную область видимости, у `let` блочная область видимости
- `+reqData.age` - знак `+` означает превратить текстовые данные в числовые 
- `let name = 'Ivan'` - присвоить `name` строку `'Ivan'`
- `let salary = 1000` - присвоить `salary` число `1000`
- `to.include()` - проверка параметра

### pm.test
- `pm.test` - Define tests using the pm.test function, providing a name and function that returns a boolean (true or false) value indicating if the test passed or failed.
- `pm.test("Status code is 200", function () {pm.response.to.have.status(200);});` - объект постмана `pm`, у которого ест метод `test`, метод принимает два аргумента: 1. Название теста; 2. функция (в частности которая проверит статус ответа)

### pm.response
- `pm.response` - Ответ сервера, 
- `const responseJson = pm.response.json();` - To parse JSON data
- `const responseJson = xml2Json(pm.response.text());` - To parse XML
- `pm.response.to.have.status(200)`
- To parse CSV, use the CSV parse (csv-parse/lib/sync) utility
```js
const parse = require('csv-parse/lib/sync');
const responseJson = parse(pm.response.text());
```
- To parse HTML, use cheerio
```js
const $ = cheerio.load(pm.response.text());
//output the html for testing
console.log($.html());
```

```js
pm.test("response should be okay to process", function () {
    pm.response.to.not.be.error;
    pm.response.to.have.jsonBody("");
    pm.response.to.not.have.jsonBody("error");
});
```
```js
pm.test("response must be valid and have a body", function () {
     pm.response.to.be.ok;
     pm.response.to.be.withBody;
     pm.response.to.be.json;
});
```

### pm.expect
- Using the `pm.expect` syntax gives your test result messages a different format. 
- 
```js
pm.test("environment to be production", function () {
    pm.expect(pm.environment.get("env")).to.equal("production");
});
```
```js
pm.test("The response has all properties", () => {
    //parse the response JSON and test three properties
    const responseJson = pm.response.json();
    pm.expect(responseJson.type).to.eql('vip');
    pm.expect(responseJson.name).to.be.a('string');
    pm.expect(responseJson.id).to.have.lengthOf(1);
});
```

- `pm.expect(respName.name).to.be.a("String")` - `to.be.a("String")` - проверить, что тип данных является строкой
- `pm.expect(resData).to.have.all.keys("age", "daily_food", "dail", "name")`
- `pm.expect(resData).to.have.any.keys("age", "daily_food", "dail", "name")`

- `const responseJson = pm.response.json()` - спарсить JSON данные
- `const responseJson = xml2Json(pm.response.text());` - спарсить XML данные
- `pm.response.text()` - ответ сервера в виде текста 
- `respSalary[0]` - достать первый элемент из массива `respSalary`

### console
- `console.log()`
- `console.log(reqData)` - отобразить в консоле значение переменной reqData
- console.info()
- console.warn()
- console.error()
- console.clear()

### Получить запрос
- `JSON.parse(pm.request.body)` - `pm.request.body` - получить данные в виде текстового запроса если типа запроса raw-JSON, `JSON.parse(` сделать из данных формат json, когда запрос в виде `raw - json`, (не считывает, если данные были отправлены в формате formdata)
- `JSON.parse(request.data)` - 
- `request.data` - получить данные запроса в виде JSON, подходит для формата formdata, для raw json данные будут в виде текста
- `pm.request` - выводит всю информцаю о запросе, в том числе данные много лишних данных
- `pm.request.body` - получить данные запроса в виде текста если тип запроса raw-JSON, при формате formdata данные практически нечитабельны
- `pm.request.body.formdata` - получить данные в формате formdata
- `pm.request.body.json()` - получить данные в формате JSON (не считывает, если данные были отправлены в формате formdata)
- `pm.request.body.raw`
- `pm.request.url.toString()` - получить url в виде строки
- `pm.request.url.query.toObject()` - получить url запроса




`request.data --> pm.request.body`



# Snippets

## Get an environment variable
Получить значение из окружения
```js
pm.environment.get("variable_key");
fromEnv = pm.environment.get("url"); // например
```

## Set an environment variable
Установить значения в окружении
```js
pm.environment.set("variable_key", "variable_value");
```
Пример использования
```js
{
    "age": 36,
    "daily_food": 0.66,
    "daily_sleep": 137.5,
    "name": "Artem"
}

let resData = pm.response.json();
pm.environment.set("sleep", resData.daily_sleep);
```


## Status code: Code is 200
```js
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});
```
- `pm.response.to.have.status(200)` - ответ сервера должен иметь статус 200

## Response body: Contains string
```js
pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("string_you_want_to_search");
});
```
Проверка наличия параметра в ответе, Пример использования:

```json
// Ответ сервера в виде json файла:
{
    "age": 25,
    "daily_food": 0.72,
    "daily_sleep": 150.0,
    "name": "Artem"
}
```

```js
// Проверка наличия параметра "name"
pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("name");
});
```


## Response body: JSON value check
```js
let jsonData = pm.response.json();
let reqData = request.data;
let reqAge = +reqData.age;

console.log(reqData)
console.log(reqAge)

pm.test("Check age", function () {
    pm.expect(jsonData.age).to.eql(reqAge);
});
```
- `var jsonData = pm.response.json();` - венесено за пределы функции, чтобы определить ответ один раз и потом этот ответ использовать в тестах
- `pm.response.json()` - получить ответ сервер и превратить в json, т.к. ответ поступает в виде текста
- `pm.expect(jsonData.age).to.eql(36)` - ожидаем что в переменной jsonData значение age будет равно 36

## Response body: is equal to a string
Сравнить полученный ответ со строкой
```js
pm.test("Body is correct", function () {
    pm.response.to.have.body("response_body_string");
});
```
Проверка наличия в тексте ответа строки "response_body_string"


```js
// Проверить, что в ответе сервера есть тип данных строка 
pm.test("Check type of data", function () {
    pm.expect(respName.name).to.be.a("String");
});
```


```js
// Проверить, что все данные есть в ответе сервера
pm.test("Check all data are in response", function () {
    pm.expect(resData).to.have.all.keys("age", "daily_food", "daily_sleep", "name");
});
```

```js
// Проверить, что хотя бы какие-либо данные есть в ответе сервера
pm.test("Check all data are in response", function () {
    pm.expect(resData).to.have.all.keys("age", "daily_food", "dail", "name");
});
```