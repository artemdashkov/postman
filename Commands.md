# Содержание
- [Теория](#Теория)
- [SOAP API](#SOAP-API)
- [Вкладки Postman](#Вкладки-Postman)
- [Environment](#Environment)
- [Run Collection](#Run-Collection)
- [Синтаксис JS](#Синтаксис-JS)
    - [циклы](#циклы)
    - [pm.test](#pmtest)
    - [pm.request](#pmrequest)
    - [pm.response](#pmresponse)
    - [pm.expect](#pmexpect)
    - [console](#console)
- [Snippets](#Snippets)

# Теория

### Ссылки на ресурсы
- chai.js documentation - https://www.chaijs.com/api/bdd/
- node.js documentation - https://www.chaijs.com/api/assert/

### Методы
- GET - retrieves data from an API.
- POST - sends new data to an API. отправляет информацию на сервер, например картинку.
- PUT - replace existing data.
- DELETE - delete existing data.
- PATCH - update some existing data fields

### Термины
- **Объект** - определение из ООП. Объект сущность, которая имеет набор свойств объекта и функций - поведение объекта. 
```js
var cat = {
    name: "Barsik",
    year: 1,

    sleep: function (){
        //sleeping code
    }
}
```
- **Объект (json-объект)** - неупорядоченное множество пар ключ:значение. (свойство объекта, ключ объекта, параметр объекта - это все одно и то же)
`{"query":"Виктор Иван", "age":7}`
- **Массив** - набор упорядоченных значений, массив заключен в квадратные скобки `fruits = ['яблоко', 'банан', 'слива']`, чтобы достать элемент массива, необходимо указать его имя и в квадратных скобках индекс элемента, например `fruits[1] = 'банан'`
- **базовый url** - например http://162.55.220.72:5005
- **endpoint** - функция, которая обрабатывает запрос, например `/user_info_2`

### Объявление переменной
- `var x = 3` , где var ключевое слово, х имя переменной
- `var name = "Jony"`
- `var response = pm.response`
- `var jsonData` или `let jsonData` - объявление переменной. у переменных `var` и `let` разная область видимости. 
    - `var` функциональную область видимости (т.е. только внутри функции), объявленная внутри функции переменная за пределами функции уже не будет видна
    - `let` блочная область видимости (т.е. внутри блока, внутри фигурных скобок), let использовать безопаснее, потому что если переменная нужна только здесь, то лучше ограничить ее область видимости
- `const` - объявление константы, которую нельзя будет в дальнейшем менять, локальная переменная, за пределами блока не будет видна
- `(никак)` - т.е. не использовать `var`, `let`, `const`, так объявляется глобальная переменная, которая будет везде видна

### Обратиться к свойству объекта
- Чтобы получить значение объекта, необходимо обращаться по ключу (указывать в квардратных скобках ключ или указывать через точку). `variable = {"query":"Виктор Иван", "age":7}`, 
    - `variable['query'] --> "Виктор Иван"`
    - `variable.query --> "Виктор Иван"` - обычно используется такая форма записи при обращении к свойству объекта

### Object.keys() - получить все ключи объекта
**Object.keys()** - позволяет получить все ключи объекта в виде массива
```js
var cat = {
    name: "Barsik",
    age: 10,
    sleep: function() {
        // sleeping code
    }
}

Object.keys(cat);
> ['name', 'age', 'sleep']
```


### Достать значение из XML
```xml
<req>
    <query attr1="value 1" attr2="value 2">Виктор Иван</query>
    <count>7</count>
</req>
```
где
- `<req>` - корневой элемент
- `<query>` - название тега = название параметра
- `attr1="value 1" attr2="value 2` - атрбиты, которые находятся внутри тегов=параметров
- `Виктор Иван` - внутри тега значение параметра
- `</query>` - закрывающий тег

Чтобы достать значения из формата xml нужно его переконвертировать в json:
- `xml2Json(responseBody)`

### Структура автотеста
- `pm.test("Status code is 200", function () {pm.response.to.have.status(200);});` - объект постмана `pm`, у которого ест метод `test`, метод принимает два аргумента: 1. Название теста; 2. функция (в частности которая проверит статус ответа)

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

- `pm.globals.set("GlobalAge", 17)` - установка глобальных переменных
- `pm.environment.set("variable_key", "variable_value")` - Установить значения в окружении
- `pm.environment.get("variable_key")` - Получить значение из окружения

# Run Collection
- `Persist responses for a session` - необходимо устанавливать чек, чтобы видеть 

# Синтаксис JS
- `var jsonData` или `let jsonData` - объявление переменной. у переменных `var` и `let` разная область видимости. у `var` функциональную область видимости (т.е. только внутри функции), у `let` блочная область видимости (т.е. внутри файла)
- `+reqData.age` - знак `+` означает превратить текстовые данные в числовые 
- `let name = 'Ivan'` - присвоить `name` строку `'Ivan'`
- `let salary = 1000` - присвоить `salary` число `1000`
- `to.include()` - проверка параметра
- `respSalary[0]` - достать первый элемент из массива `respSalary`
- `typeof variable` - узнать тип переменной variable 
- `&&` - логическое и
- `||` - логическое или
- `count++` - прибавить к count 1
- \ - экранирование
- // или /* */ - комментарии
- `=` присваивает значение
- `==` равенство с приведением типов ("2" = 2)
- `===` строгое равенство ("2" != 2)
```js
if (true){
    console.log("выполняется код")
}

if (false){
    console.log("код выполняться не будет")
}

if (false){
    console.log("код выполняться не будет")
} else if (false){
    console.log("код выполняться не будет")
} else {
    console.log("выполняется код")
}
```

## Циклы
### while
```js
let count = 0
while (true){
    console.log('While', count)
    count++     
}
```
### for
```js
for (let i=0; i < 10; i++){
    console.log("For i --> ", i)
}
```

```js
// выводит последовательно все значения из i 
let i = [0, 3, 2, [9, 4], 'string']
for (let n of i){
    console.log(n)
}
```


## pm.test
- `pm.test` - Define tests using the pm.test function, providing a name and function that returns a boolean (true or false) value indicating if the test passed or failed.
- `pm.test("Status code is 200", function () {pm.response.to.have.status(200);});` - объект постмана `pm`, у которого ест метод `test`, метод принимает два аргумента: 1. Название теста; 2. функция (в частности которая проверит статус ответа)

## pm.request
- `JSON.parse(pm.request.body.raw)`, где `pm.request.body` - получить данные в виде текстового запроса если типа запроса raw-JSON, `JSON.parse(` сделать из данных формат json, когда запрос в виде `raw - json`, (не считывает, если данные были отправлены в формате formdata)
- `JSON.parse(JSON.stringify(pm.request.body.formdata))` 
    - `JSON.parse(JSON.stringify())` technique is used to create a deep copy of an object in JavaScript. 
    - `JSON.stringify()` function converts JavaScript objects into JSON strings.
    - `JSON.parse()` - This converts the JSON string back into a JavaScript object.
- `JSON.parse(request.data)` - 
- `request.data` - получить данные запроса в виде JSON, подходит для формата formdata, для raw json данные будут в виде текста. метод считается устраевшим, но работает
- `pm.request` - выводит всю информцаю о запросе, в том числе данные много лишних данных
- `pm.request.body` - получить данные запроса в виде текста если тип запроса raw-JSON, при формате formdata данные практически нечитабельны. The data in the request body. This object is immutable and can't be modified from scripts.

- `pm.request.body.formdata` - получить данные в формате formdata
    [{…}, {…}, {…}, {…}]
- `pm.request.body.formdata[1]` - получить данные в формате formdata
    undefined

- `JSON.parse(JSON.stringify(pm.request.body.formdata))` 
    [{…}, {…}, {…}, {…}]
- `JSON.parse(JSON.stringify(pm.request.body.formdata))[1]` 
    {key: "weight", value: "100", type: "text"}
- `JSON.parse(JSON.stringify(pm.request.body.formdata))[1].value` 
    "100"

- `pm.request.body.json()` - получить данные в формате JSON (не считывает, если данные были отправлены в формате formdata)
- `pm.request.body.raw`
- `pm.request.headers:HeaderList` - The list of headers for the current request
- `pm.request.method:String` - The HTTP request method:
- `pm.request.url` - The request URL
- `pm.request.url.toString()` - получить url в виде строки
- `pm.request.url.query.toObject()` - получить url запроса в виде объекта JS
- `pm.request.url.query.toObject().salary` - получить из GET Запроса значение salary
```js
const requestData = JSON.parse(JSON.stringify(pm.request.body.formdata))
console.log(requestData[0])
```

```js
{{url}}/object_info_3?name=Ivan&age=17&salary=1000
var requestData = pm.request.url.query.toObject()
console.log(requestData)

{name: "Ivan", age: "17", salary: "1000"}
```

## pm.response
- `pm.response` - Ответ сервера, объект, который содержит много дополнительной информации ответа
- `pm.response.json()` - To parse JSON data, объект.
- `pm.response.text()` - преобразовывает ответ сервера в виде текста, тип данных string 
- `xml2Json(pm.response.text())` - To parse XML
- `xml2Json(responseBody)` - To parse XML
- `pm.response.to.be.info` - Check 1XX status code 
- `pm.response.to.be.success` - Check 2XX status code 
- `pm.response.to.be.redirection` - Check 3XX status code
- `pm.response.to.be.clientError` - Check 4XX status code
- `pm.response.to.be.serverError` - Check 5XX status code
- `pm.response.to.be.error` - Check 4XX or 5XX status code
- `pm.response.to.be.ok` - Check 2XX status code
- `pm.response.to.be.withBody`
- `pm.response.to.be.json`
- **pm.response.to.have.body()** - сравнивает ответ сервера в виде объекта с со строкой, например `pm.response.to.have.body("User-agent: *\nDisallow: /deny\n")`
- `pm.response.to.have.jsonBody("")`
- `pm.response.to.have.status(200)`
- `pm.response.to.not.have.status(200)` - не является статусом 200
- `pm.response.to.not.be.error`
- `pm.response.to.not.have.jsonBody("error");`

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

## pm.expect
все что написано после функции pm.expect() выполняться не будет
- `pm.expect` syntax gives your test result messages a different format. 
- `pm.expect(resData).to.be.a("String")` - `to.be.a("String")` - проверить, что тип данных является строкой, вместо "String" можно использовать "Number", null - пустота, "undefined" - неизвестная переменная
- `pm.expect(resData.code == 200).to.be.true`
- `pm.expect(resData.code == 200).to.be.false`
- `pm.expect(pm.response.code).to.be.at.least(200)` - ожидаемое значение больше либо равно 200
- `pm.expect(pm.response.code).to.be.at.most(200)` - ожидаемое значение меньше либо равно 200
- `pm.expect(pm.response.code).to.be.above(199)` - проверить, что код ответа больше 199
- `pm.expect(pm.response.code).to.be.below(201)` - проверить, что код ответа меньше 201
- `pm.expect(pm.response.code).to.be.within(200, 205)` - ожидаемое значение должно быть в диапазоне от 200 до 205
- `pm.expect(pm.response.code).to.be.closeTo(202, 1)` - ожидаемый ответ в диапазоне от 201 до 203
- `pm.expect(pm.response.code).to.match(/^200/)` - сравнение с использованием регулярных выражений
- `pm.expect(null).to.be.null`
- `pm.expect(null).to.be.NaN` - Проверка чисел
- `pm.expect(resData).to.eql(200)` - проверить соответствие
- `pm.expect(resData).to.equal(200)` - проверить соответствие, принебрегает тип данных, строка и номера будут равны
- `pm.expect(resData).to.equal(200)` - проверить соответствие, учитывает тип данных, строка и номера будут не равны
- `pm.expect(resData).to.include("any_string")` - проверить наличие строки в ответе
- `pm.expect(resData).to.have.all.keys("age", "daily_food", "dail", "name")` - проверить, что есть все существующие свойства в ответе есть в скобках
- `pm.expect(resData).to.have.any.keys("age", "daily_food", "dail", "name")` - проверить, что все свойства в скобках есть в ответе
- `pm.expect(resData).to.have.string("age")` - проверить, что ответ имеет строку "age"


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


## console
- `console.log()`
- `console.log(reqData)` - отобразить в консоле значение переменной reqData
- `console.info()`
- `console.warn()`
- `console.error()`
- `console.clear()`




`request.data --> pm.request.body`

# Snippets

## Get an environment variable
Получить значение из окружения
```js
pm.environment.get("variable_key");
fromEnv = pm.environment.get("url"); // например
```
pm.variables.get(""); // ENTER VALUE HERE

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

## Status code: Successful POST request
Используется для проверки статуса из списка
```js
pm.test("Successful POST request", function () {
    pm.expect(pm.response.code).to.be.oneOf([201, 202]);
});
```

## Status code: Code name has string
Проверка статуса
```js
pm.test("Status code name has string", function () {
    pm.response.to.have.status("OK");
});
```

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

# Тесты на Body: Text
Snippet. Response body: Is equal to a string
```js
pm.test("Body is correct", function () {
    pm.response.to.have.body("User-agent: *\nDisallow: /deny\n");
});
// pm.response - ответ сервера
// to.have.body - проверяет все тело целиком
// "User-agent: *\nDisallow: /deny\n" - текст, который должен содержать ответ сервера, необходимо учитывать перенос строки \n
```

Snippet. Response body: Contains string
```js
pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("User-agent");
});
// to.include - содержит
```
Проверка соответствия
```js
pm.test("Body через to.be.true", function () {
    pm.expect(pm.response.text() == "User-agent: *\nDisallow: /deny\n").to.be.true;
});
// to.include - содержит
```
Проверка длины
```js
pm.test("Проверка длины", function () {
    pm.expect(pm.response.text()).to.have.lengthOf(30);
});
// to.have.lengthOf(30) - длина строки 30
```
Проверка длины больше 20
```js
pm.test("Проверка длины больше 20", function () {
    pm.expect(pm.response.text()).to.have.lengthOf.above(20);
});
```
Проверка существования текста
```js
pm.test("Проверка существования текста", function () {
    pm.expect(pm.response.text()).to.exist;
});
```
Проверка отсутствия текста
```js
pm.test("Проверка отсутствия текста", function () {
    pm.expect(pm.response.text()).to.not.exist;
});
```
Проверка что текст не пустой
```js
pm.test("Проверка что текст не пустой", function () {
    pm.expect(pm.response.text()).to.not.be.empty;
});
```

# Тесты на Body: JSON
```js
pm.test("Your test name", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.value).to.eql(100);
});
```