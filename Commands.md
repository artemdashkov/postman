## Синтаксис JS
- `var jsonData` или `let jsonData` - объявление переменной. у переменных `var` и `let` разная область видимости. у `var` функциональную область видимости, у `let` блочная область видимости
- `pm.test("Status code is 200", function () {pm.response.to.have.status(200);});` - объект постмана `pm`, у которого ест метод `test`, метод принимает два аргумента: 1. Название теста; 2. функция (в частности которая проверит статус ответа)
- `pm.response` - Ответ сервера
- `pm.response.text()` - ответ сервера в виде текста 
- `console.log(reqData)` - отобразить в консоле значение переменной reqData
- `JSON.parse(pm.request.body)` - `pm.request.body` - получить данные в виде текстового запроса если типа запроса raw-JSON, `JSON.parse(` сделать из данных формат json, когда запрос в виде `raw - json`
- `JSON.parse(request.data)` - 
- `request.data` - получить данные запроса в виде JSON, подходит для формата formdata, для raw json данные будут в виде текста
- `pm.request.body` - получить данные запроса в виде текста если тип запроса raw-JSON
- `pm.request.body.formdata` - получить данные в формате formdata
- `pm.request.body.toJSON` - получить данные в формате JSON
- `pm.request.url.query.toObject()` - получить url запроса
- `+reqData.age` - знак `+` означает превратить текстовые данные в числовые 
- `to.include()` - проверка параметра


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
