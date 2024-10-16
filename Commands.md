# Содержание
- [Теория](#Теория)
- [SOAP API](#SOAP-API)
- [Вкладки Postman](#Вкладки-Postman)
- [Environment](#Environment)
- [Run Collection](#Run-Collection)
- [Синтаксис JS](#Синтаксис-JS)
    - [конструктор](#конструктор)
    - [циклы](#циклы)
    - [pm.test](#pmtest)
    - [pm.request](#pmrequest)
    - [pm.response](#pmresponse)
    - [pm.expect](#pmexpect)
    - [console](#console)
- [Snippets](#Snippets)
- [Тесты на Body-Text](#Тесты-на-Body-Text)
- [Тесты на Body-JSON](#Тесты-на-Body-JSON)
    - [iclude vs deep.include](#iclude-vs-deepinclude)
    - [.nested](#nested)
    - [property, keys](#Тесты-на-свойстваключи-объекта-в-Postman---property-keys-Chaijs)
    - [.nested](#nested)
    - [Типы элементов](#Типы-элементов)
    - [Тесты на массивы](#Тесты-на-массивы)
    
# Теория

### Ссылки на ресурсы
- chai.js documentation, библиотека, которая расширяет функционал тестов 
    - https://www.chaijs.com/api/bdd/
    - https://www.chaijs.com/api/assert/
- node.js documentation - https://nodejs.org/docs/latest/api/assert.html 

### Методы
- GET - используется для получения данных, retrieves data from an API. не имеет body (тело запроса).
- POST - sends new data to an API. отправляет информацию на сервер, например картинку или текст, создает ресур по отправленным данным - например пользователя.
- PUT - replace existing data (заменяет полностью содержимое существующей сущнсоти). Если через PUT передадим не все поля, оставят непереданные поля null. К объекту необходимо обратиться по id. Похож на POST запрос, только обновляет данные у уже созданного объекта. 
- DELETE - delete existing data. Надо обратить по ID
- PATCH - update some existing data fields. Похож на метод PUT, но удобнее, т.к. обновляет только те данные, которые мы передаем и которые хотим поменять.
- UPDATE

- Path-параметры: путь к ресурсу, является частью url, меняющася часть url, например id пользователя "any-site.com/users/15df4s"
- query-параметры: фильтрации, перечисления свойств, тоже является частью url, но передается как параметр 

### Ответ сервера
- `1ХХ`   Информационные (редко можно увидеть), это статусы с помощью которых сервисы обмениваются между собой информационными сообщениями

- `2ХХ`   Успешные статусы когда все пошло хорошо. 
- 200	Запрос успешно обработан
- 201   Created. все что ты создавал создалось,

- `3ХХ`   Перенаправления

- `4XX` Клиентские ошибки, значит мы ошиблись с запросом и в запросе отправили что то не то
- 400	Некорректный запрос (невалидный JSON или XML)
- 401	В запросе отсутствует API-ключ
- 403	нет прав доступа к ресурсу, В запросе указан несуществующий API-ключ. 
    - Или не подтверждена почта
    - Или исчерпан дневной лимит по количеству запросов
- 404   ресурс не найден
- 405	Запрос сделан с методом, отличным от POST
- 413	Слишком большая длина запроса или слишком много условий
- 429	Слишком много запросов в секунду или новых соединений в минуту

- `5xx`	Серверная ошибка, произошла внутренняя ошибка сервиса, т.е. на стороне сервера есть какие-то проблемы 

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
- **Headers** - служебная информация о том, что мы конкретно отправляем серверу и как сервер себя ведет
    - `"Content-Type"`: `"multipart/form-data; boundary=<calculated when request is sent>"` - какой типа данных мы отправляем, может быть `application/xml`, `text/plain`, `application/json`
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
- создание массива
    - var items = new Array(); например `var nums = new Array(1, 2, 3);`
    - var items = []; например `var nums = [1, 2, 3];` - литеральный синтаксис
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
## Конструктор
```js
// создаем конструктор
function Cat (name, year) {
    this.name = name;
    this.year = year;
    this.sleep = function() {
        //sleeping code
    }
}
// создаем объект на основе конструктора
var barsik = new Cat("Barsik", 1)
// barsik - имя переменной; new - ключевое слово, которое говорит, что будем создавать новый объект с помощью конструктора; Cat - имя конструктора; ("Barsik", 1) - аргументы функции: имя и возраст
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
- `pm.expect(resData).to.be.a("String")` - `to.be.a("String")` - проверить, что тип данных является строкой, вместо "String" можно использовать "Number" - число, null - пустота, "undefined" - неизвестная переменная, "array" - массив, "object" - объект
- `pm.expect(resData.code == 200).to.be.true` - позволяет выполнять не строгое равенство, строка будет равна числу
- `pm.expect(resData.code == 200).to.be.ok` - позволяет выполнять не строгое равенство, строка будет равна числу
- `pm.expect(resData.code == 200).to.be.false`
- `pm.expect(pm.response.code).to.be.at.least(200)` - ожидаемое значение больше либо равно 200
- `pm.expect(pm.response.code).to.be.at.most(200)` - ожидаемое значение меньше либо равно 200
- `pm.expect(pm.response.code).to.be.above(199)` - проверить, что код ответа больше 199
- `pm.expect(pm.response.code).to.be.below(201)` - проверить, что код ответа меньше 201
- `pm.expect(pm.response.array).to.be.empty` - проверить, что массив array пустой
- `pm.expect(pm.response.code).to.be.within(200, 205)` - ожидаемое значение должно быть в диапазоне от 200 до 205
- `pm.expect(pm.response.code).to.be.closeTo(202, 1)` - ожидаемый ответ в диапазоне от 201 до 203
- `pm.expect(pm.response.code).to.match(/^200/)` - сравнение с использованием регулярных выражений
- `pm.expect(null).to.be.null`
- `pm.expect(null).to.be.NaN` - Проверка чисел
- `pm.expect(resData).to.eql(200)` - проверить полное равенство (===), позвояляет сравнивать объекты. вместо `.eql` можно использовать `.eqls` 
- `pm.expect(resData).to.equal(200)` - проверить полное равенство (===), учитывает тип данных, строка и номера будут не равны, позволяет сравнивать **только** значения. вместо `.equal` можно использовать `.equals` и `.eq`
- `pm.expect(resData).to.deep.equal(200)` работает аналогичным образом как и `pm.expect(resData).to.eql(200)`
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
Проверяем равенство значения через eql - позволяет сравнивать объекты/equal - позволяет сравнивать только значение
```js
var jsonData = pm.response.json();

pm.test("Тест через eql", function () {
    pm.expect(jsonData.value).to.eql(100);
});

pm.test("Тест через equal", function () {
    pm.expect(jsonData.value).to.equals(100);
});
// проверяем, что значение value в ответе равно 100
```
Проверяем равенство через deep.equal - позволяет сравнивать не только значения, но и объекты, массивы и т.д.
```js
pm.test("Тест на type через deep.equal", function () {
    pm.expect(jsonData).to.deep.equal({
    "type": "error",
    "message": " email test_demo_2@gmail.com уже есть в базе"
});
});

pm.test("Тест на type через deep.equal", function () {
    pm.expect(jsonData).to.deep.equal({
    "type": "error",
    "message": " email test_demo_2@gmail.com уже есть в базе"
}).and.equal({
    "type": "error",
    "message": " email test_demo_2@gmail.com уже есть в базе"
});
});
// deepl.equal в отличии от equal может работать в цепочке
```

Проверяем тело ответа
```js
pm.test("Body is correct", function () {
    pm.response.to.have.body("response_body_string");
});
// response_body_string - можно скопировать полностью ответа в виде json. вставлять без кавычек
```
Проверяем наличие текста в ответе
```js
pm.test("Body matches string", function () {
    pm.expect(pm.response.text()).to.include("3000");
});

// учитывать, что текст на русском языке при конвертации из json в txt перекодировывается
```
Проверяем наличие текста в ответе с помощью include
```js
var jsonData = pm.response.json();

pm.test("Your test name", function () {
    pm.expect(jsonData.message).to.include("email");
});
// ищем вхождение слова в значение message

pm.test("to.include", function () {
    pm.expect(jsonData).to.include({"message": " email test_demo_2@gmail.com уже есть в базе"});
});
// вхождение объекта в json ответ
```
## iclude vs deep.include
**include** - вхождение элемента, используется для простой строки/массива, т.е. полученного на первом уровне вложенности

**deep.include** - вхождение элемента или дочернего объекта, если хотим залезть на уровень ниже по дереву ответа, при тестировании дочерених объектов будет срабатывать только `deep.include`

Пример 1
```js
{
    "type": "error",
    "message": " email test_demo_2@gmail.com уже есть в базе"
}
pm.expect(jsonData.type).to.include("error"); // будет работать
pm.expect(jsonData.type).to.deep.include("error"); // будет работать
```
Пример 2
```js
{
    "type": "error",
    "data": {"message":"MSG"}
}
pm.expect(jsonData.data).to.include({"message":"MSG"}); // будет работать
pm.expect(jsonData.data).to.deep.include({"message":"MSG"}); // будет работать
```
Пример 3
```js
{
    "type": "error",
    "data": {"message":"MSG"}
}
pm.expect(jsonData).to.deep.include({data:{"message":"MSG"}}); //если мы хотим убедиться в наличии целого дочернего элемента, то будет работать только `deep.include`
```

## .nested
позволяет писать путь к тестируемому объекту в дот натации (если мы хотим пройтись по дереву объекта)
```js
{a:{b:['x','y']}}

pm.expect(jsonData.a.b[1]).to.include('y');
pm.expect(jsonData).to.nested.include({'a.b[1]':'y'});
```
Пример 
```js
pm.expect(jsonData).to.have.nested.property('suggestions[0].data.gender', 'FEMALE');
```

## Тесты на свойства=ключи объекта в Postman - (property, keys: Chai.js)
property - не может идти на уровень ниже и найти свойства объекта, находящегося внутри нашего объекта
deep.property - используется для тестирования дочернего элемента, может идти на уровень ниже и найти свойства объекта, находящегося внутри нашего объекта, но в pm.expect() необходимо передавать имеено тот объект, в котором находится свойство
.nested - поиск по дереву
```js
var jsonData = pm.response.json();

pm.test("Check property message", function () {
    pm.expect(jsonData).to.have.property('message');
});
// to.have.property('message') - проверка свойства/ключа 'message'
```
```js
pm.test("Проверить все свойства", function () {
    pm.expect(jsonData).to.have.property('type');
    pm.expect(jsonData).to.have.property('message'); // при этом, если упадет первый тест, то второй уже выполняться не будет
});
// способ для проверки нескольких или всех свойств
```
Проверить свойства и значение
```js
pm.test("Проверить свойства и значение", function () {
    pm.expect(jsonData).to.have.property('message', ' email test_demo_2@gmail.com уже есть в базе');
});
// можно проверить одновременно наличие свойства и его значения
```
Проверить, что значение свойства является числом
```js
pm.test("Проверка to.have.property u_age есть число", function () {
    pm.expect(respData.person).to.have.property("u_age").that.is.a('number');
});
```
Проверить, что значение свойства является строкой
```js
pm.test("Проверка to.have.property u_age есть строка", function () {
    pm.expect(respData.person).to.have.property("u_age").that.is.a('string');
});
```
Проверить наличие всех свойств=ключей
```js
pm.test("Проверить наличие всех свойств=ключей", function () {
    pm.expect(jsonData).to.have.all.keys('type', 'message');
});
```
Проверить наличие одного свойства
```js
pm.test("Проверить наличие одного свойства", function () {
    pm.expect(jsonData).to.have.any.keys('type', 'messag7e');
});
```
Поиск по дереву 
```js
pm.expect(jsonData).to.have.nested.property('suggestions[0].data.gender', 'FEMALE');
```

## Типы элементов
Chai.js: .a (an), .instanceof

.instanceof - оператор, который позволяет проверить, создан ли объект данной функцией, работает для любых функций - как встроенных, так и наших. Используется для того, чтобы проверить что дочерний элемент является объектом.
пример:
 - `pm.expect(jsonData.suggestions[0].data).to.be.an.instanceof(Object)`
 - `pm.expect(jsonData.companys).to.be.an.instanceof(Array)`

.a (an) - выполняет аналогичные функции, что и .instanceof. Проще использовать .a (an), т.к. запись выглядит проще и понятнее.
- `pm.expect(jsonData.type).to.be.a('string')`

проверка, что тип данных строка
```js
pm.test("type - это строка", function () {
    pm.expect(jsonData.type).to.be.a('string');
});
```
проверка, что тип данных объект
```js
pm.test("jsonData - это объект", function () {
    pm.expect(jsonData).to.be.a('object');
});
```
проверка, что тип данных массив
```js
pm.test("jsonData - это массив", function () {
    pm.expect(jsonData).to.be.an('array');
});
```
проверка, что тип данных число
```js
pm.test("jsonData - это массив", function () {
    pm.expect(jsonData.tasks[0].id_task).to.be.a('number');
});
```
Цепочка проверок
```js
pm.test("Цепочка проверок", function () {
    pm.expect(jsonData.tasks).to.be.an('array').that.deep.includes({
        "name":"Новая",
        "id_task":32
    });
});
```
Свойство объекта - это число
```js
pm.test("Свойство объекта - это число", function () {
    pm.expect({a: 1}).to.have.property('a').that.is.a('number');
});
```
Проверка что, объект создан с помощью конструктора
```js
function Cat () {}
pm.expect(new Cat()).to.be.an.instanceof(Cat);
// Проверка что, объект "new Cat()" создан с помощью конструктора Cat
```
Проверка что, массив является массивом
```js
pm.expect([1, 2]).to.be.an.instanceof(Array);
// Проверка что, массив является массивом
```

## Тесты на массивы
Проверки на существование массива
- pm.expect(jsonData.companys).to.not.be.null;
- pm.expect(jsonData.companys).to.not.be.empty;
- pm.expect(jsonData.companys).to.not.be.exist; // Не рекомендуется использовать

### Использование to.be.a('array')
Проверка, что элемент ответа является массивом
```js
var jsonData = pm.response.json();

pm.test("companys - это массив", function () {
    pm.expect(jsonData.companys).to.be.a('array');
});
```

Проверить что массив пустой/ не пустой
```js
pm.test("Проверить, что массив пустой", function () {
    pm.expect(jsonData.companys).to.be.a('array').that.is.empty;
});

pm.test("Проверить, что массив не пустой", function () {
    pm.expect(jsonData.companys).to.be.a('array').that.is.not.empty;
});
```
Проверить наличие элемента внутри массива
```js
pm.test("Проверить, что массив не включает в себя элемент", function () {
    pm.expect(jsonData.companys).to.be.a('array').that.deep.include({
            "name": "Ромашка",
            "id_company": 7
        });
});
```
### Использование .instanceof
```js
pm.test("companys - это массив, проверка с помощью .instanceof", function () {
    pm.expect(jsonData.companys).to.be.an.instanceof(Array);
});
```
### Использование to.have.all.keys
Проверка ключей - to.have.all.keys
```js
pm.test("Проверка ключей - to.have.all.keys", function () {
    pm.expect({a: 1, b: 2}).to.have.all.keys('a', 'b');
});
```
Массив имеет все 3 элемента
```js
pm.test("Массив имеет 3 элемента", function () {
    pm.expect(jsonData.companys).to.have.all.keys(0, 1, 2);
});
```
### Использование to.have.any.keys
Массив имеет хотя бы какой-либо элемент
```js
pm.test("Массив any.keys", function () {
    pm.expect(jsonData.companys).to.have.any.keys(0, 1, 100);
});
```
### Использование .members
Массив имеет все ключи .members
```js
pm.test("Массив имеет все ключи .members", function () {
    pm.expect([1, 2, 3]).to.have.members([2, 1, 3]);
});

pm.test("Массив имеет все ключи .members", function () {
    pm.expect(["test", 2, 3]).to.have.members([2, 'test', 3]);
});
```
Массив включает перечисленные элементы
```js
pm.test("Массив включает перечисленные элементы", function () {
    pm.expect(["test", 2, 3]).to.include.members([2, 3]);
});
```
Проверка реального массива
```js
pm.test("have.deep.members", function () {
    pm.expect(jsonData.companys).to.have.deep.members([
        {
            "name": "Алкоголики и тунеядцы",
            "id_company": 15
        },
        {
            "name": "Петушки",
            "id_company": 8
        },
        {
            "name": "Ромашка",
            "id_company": 7
        }
    ]);
});
```
Проверка реального массива с учетом порядка
```js
pm.test("have.deep.members - с учетом порядка", function () {
    pm.expect(jsonData.companys).to.have.deep.ordered.members([
        {
            "name": "Алкоголики и тунеядцы",
            "id_company": 15
        },
        {
            "name": "Петушки",
            "id_company": 8
        },
        {
            "name": "Ромашка",
            "id_company": 7
        }
    ]);
});
```

### Использование .lengtOf
.lengtOf - проверка длины массива. .lengthOf can also be used as a language chain, causing all .above, .below, .least, .most, and .within
```js
pm.test("to.have.lengthOf == 3", function () {
    pm.expect(jsonData.companys).to.have.lengthOf(3);
});
```
ИНформация из оф документации
```js
// Recommended
expect([1, 2, 3]).to.have.lengthOf(3);

// Not recommended
expect([1, 2, 3]).to.have.lengthOf.above(2);
expect([1, 2, 3]).to.have.lengthOf.below(4);
expect([1, 2, 3]).to.have.lengthOf.at.least(3);
expect([1, 2, 3]).to.have.lengthOf.at.most(3);
expect([1, 2, 3]).to.have.lengthOf.within(2,4);
```

## Строка в JSON
- **Проверка типа данных**
    - `pm.expect(jsonData.type).to.be.a('string');`
- **Проверка данных**
    - `pm.expect(jsonData.type).to.equal('error');`
    - `pm.expect(jsonData.type).to.include('rr');`
- **Проверка длины:**
    - `pm.expect(jsonData.companys[0].name).to.have.lengthOf(33);`
- **Проверка равенства:**
    - `pm.expect(jsonData.message === "Успех!").to.be.ok;`
    - `pm.expect(jsonData.message === "Успех!").to.be.true;`
- **Проверка существования строки:**
    - `pm.expect(jsonData.message).to.not.be.null;`
    - `pm.expect(jsonData.message).to.not.be.empty;`
    - `pm.expect(jsonData.message).to.not.exist;`

## Число в JSON
- **Проверка элемента, что он является числом**
    - `pm.expect(jsonData.tasks[0].id_task).to.be.a('number');`
- **Проверка равенства**
    - `pm.expect(jsonData.tasks[0].id_task).to.equal(100);` !!! самый полезный тест для чисел в JSON
- **Проверка вхождения**
    - ~~`pm.expect(jsonData.tasks[0].id_task).to.include(99);`~~ include для чисел не работает 

    - `pm.expect(jsonData.tasks[0].id_task).to.be.oneOf([201,200,202]);`
    - `pm.expect(jsonData.tasks[0].id_task).to.be.below(210);`
    - `pm.expect(jsonData.tasks[0].id_task).to.be.above(10);`
    - `pm.expect(jsonData.tasks[0].id_task).to.at.least(210);`
    - `pm.expect(jsonData.tasks[0].id_task).to.at.most(10);`
    - `pm.expect(jsonData.tasks[0].id_task).to.at.within(1, 10);`

    - `pm.expect(num === 33).to.be.ok;`
    - `pm.expect(num === 33).to.be.true;`
- **Проверка существования числа:**
    - `pm.expect(jsonData.message).to.not.be.null;`
    - ~~`pm.expect(jsonData.message).to.not.be.empty;`~~
    - `pm.expect(jsonData.message).to.not.be.undefined;`
    - `pm.expect(jsonData.message).to.not.be.NaN;`
    - `pm.expect(jsonData.message).to.not.exist;`

## Тесты на body: XML
Чтобы достать значения из формата xml нужно его переконвертировать в json:
```js
var jsonObject = xml2Json(responseBody);
// xml2Json - функция, которая преобразует xml в json
// responseBody - Ответ от сервера в формате xml
```
Далее работаем аналогичным образом, как и с Json. НЕобходимо учесть, что при конвертации XML в JSON данные могут немного искажаться. Например при ответе сервера в Json для отсутствующих данных указывается `null`, в xml `""`

## Тесты на заголовки
Сниппет Response headers: Content-Type header check. Проверка наличия заголовка
```js
pm.test("Content-Type is present", function () {
    pm.response.to.have.header("Content-Type");
});
// pm.response - ответ от сервера
// to.have.header("Content-Type") - имеет заголовок "Content-Type"
```

Проверка наличия заголовка и его значени
```js
pm.test("Content-Type is present", function () {
    pm.response.to.have.header("Content-Type", "application/json");
});
```