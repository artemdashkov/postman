## 1. POST запрос /login
http://162.55.220.72:5005/login
- login : str (кроме /)
- password : str

Что нужно сделать: 
- необходимо залогиниться
- Приходящий токен необходимо передать во все остальные запросы. дальше все запросы требуют наличие токена.

## 2. POST запрос /user_info

http://162.55.220.72:5005/user_info
- req. (RAW JSON)
- POST
- age: int
- salary: int
- name: str
- auth_token
```
resp.
{'start_qa_salary':salary,
 'qa_salary_after_6_months': salary * 2,
 'qa_salary_after_12_months': salary * 2.9,
 'person': {'u_name':[user_name, salary, age],
                                'u_age':age,
                                'u_salary_1.5_year': salary * 4}
                                }
```
### 1. Статус код 200
### 2. Проверка структуры json в ответе.
```js
var respData = pm.response.json()
var schema = {
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "person": {
      "type": "object",
      "properties": {
        "u_age": {
          "type": "integer"
        },
        "u_name": {
          "type": "array",
          "items": [
            {
              "type": "string"
            },
            {
              "type": "integer"
            },
            {
              "type": "integer"
            }
          ]
        },
        "u_salary_1_5_year": {
          "type": "integer"
        }
      },
      "required": [
        "u_age",
        "u_name",
        "u_salary_1_5_year"
      ]
    },
    "qa_salary_after_12_months": {
      "type": "number"
    },
    "qa_salary_after_6_months": {
      "type": "integer"
    },
    "start_qa_salary": {
      "type": "integer"
    }
  },
  "required": [
    "person",
    "qa_salary_after_12_months",
    "qa_salary_after_6_months",
    "start_qa_salary"
  ]
}

pm.test('Schema is valid', function () {
    pm.expect(tv4.validate(respData, schema)).to.be.true;
});

console.log(tv4.error)
```
or
```json
pm.test('Validate the schema json', function () {
    pm.expect(RespData).to.have.jsonSchema(schema);
   
});
```

### 3. В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения на коэффициент.
var koef_1 = 4;
var koef_1 = 2.9;
var koef_1 = 2;
var koef_1 = 1;

pm.test("Check coefficients_1", function() {
    pm.expect(respData.person.u_salary_1_5_year).to.eql(respData.person.u_name[1]*4)
});

pm.test("Check coefficients_2", function() {
    pm.expect(respData.qa_salary_after_12_months).to.eql(respData.person.u_name[1]*2.9)
});

pm.test("Check coefficients_3", function() {
    pm.expect(respData.qa_salary_after_6_months).to.eql(respData.person.u_name[1]*2)
});

pm.test("Check coefficients_4", function () {
pm.environment.set("variable_key", "variable_value");
    pm.expect(respData.start_qa_salary).to.eql(respData.person.u_name[1]*1)
});

### 4. Достать значение из поля 'u_salary_1.5_year' и передать в поле salary запроса http://162.55.220.72:5005/get_test_user
var u_salary = respData.person.u_salary_1_5_year
pm.environment.get('salary' = u_salary)

в "Body - form data" добавить: Key - salary, Value - {{salary}}

# 3. POST запрос /new_data
- http://162.55.220.72:5005/new_data
- req.
- POST
- age: int
- salary: int
- name: str
- auth_token

Тесты:
## +1. Статус код 200
## +2. Проверка структуры json в ответе.
```
{'name':name,
  'age': int(age),
  'salary': [salary, str(salary*2), str(salary*3)]}
```
Get pespons

`var pespData = pm.response.json()`

Create JSON Schema
```json
{
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "age": {
      "type": "integer"
    },
    "name": {
      "type": "string"
    },
    "salary": {
      "type": "array",
      "items": [
        {
          "type": "integer"
        },
        {
          "type": "string"
        },
        {
          "type": "string"
        }
      ]
    }
  },
  "required": [
    "age",
    "name",
    "salary"
  ]
}
```
Validate data:
```js
pm.test('Schema is valid', function () {
    pm.expect(tv4.validate(pespData, schema)).to.be.true;
});
```
## 3. В ответе указаны коэффициенты умножения salary, напишите тесты по проверке правильности результата перемножения на коэффициент.
```js
var pespData = pm.response.json()
const salary = 1200
var coef_1 = 1
var coef_2 = 2
var coef_3 = 3
pm.test('Test coef_1', function () {
    pm.expect(pespData.salary[0]/salary).to.eql(coef_1)
});

pm.test('Test coef_2', function () {
    pm.expect(pespData.salary[1]/salary).to.eql(coef_2)
});

pm.test('Test coef_3', function () {
    pm.expect(pespData.salary[2]/salary).to.eql(coef_3)
});
```

## 4. проверить, что 2-й элемент массива salary больше 1-го и 0-го
```js
pm.test("Check item salary 2 > 1 & 2 > 0", function () {
    pm.expect(+pespData.salary[2]).to.be.above(+pespData.salary[1]);
    pm.expect(+pespData.salary[2]).to.be.above(+pespData.salary[0]);
});
```

# 4. POST запрос /test_pet_info
```
http://162.55.220.72:5005/test_pet_info

POST
age: int
weight: int
name: str
auth_token
```
```
Resp.
{'name': name,
 'age': age,
 'daily_food':weight * 0.012,
 'daily_sleep': weight * 2.5}
```

## Тесты:
### 1. Статус код 200
### 2. Проверка структуры json в ответе.
```js
var respData = pm.response.json();
var schema = {
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "age": {
      "type": "integer"
    },
    "daily_food": {
      "type": "number"
    },
    "daily_sleep": {
      "type": "number"
    },
    "name": {
      "type": "string"
    }
  },
  "required": [
    "age",
    "daily_food",
    "daily_sleep",
    "name"
  ]
}

pm.test('Check schema is valid', function () {
    pm.expect(tv4.validate(respData, schema)).to.be.true;
});
```

### 3. В ответе указаны коэффициенты умножения weight, напишите тесты по проверке правильности результата перемножения на коэффициент.
```js
var respData = pm.response.json();
var reqData = pm.request.body.formdata;
var reqJSON = JSON.parse(JSON.stringify(reqData));

var coef_1 = 0.012
var coef_2 = 2.5

pm.test('Check weight coeff.', function () {
    pm.expect(respData.daily_sleep).to.eql((+reqJSON[1].value)*coef_2)
});
```

# 5. POST запрос /get_test_user
```
http://162.55.220.72:5005/get_test_user

POST
age: int
salary: int
name: str
auth_token

Resp.
{'name': name,
 'age':age,
 'salary': salary,
 'family':{'children':[['Alex', 24],['Kate', 12]],
 'u_salary_1.5_year': salary * 4}
  }
```
## Тесты:
### 1) + Статус код 200
### 2) Проверка структуры json в ответе.
```js
var respData = pm.response.json()

var schema = {
  "$schema": "http://json-schema.org/draft-04/schema#",
  "type": "object",
  "properties": {
    "age": {
      "type": "string"
    },
    "family": {
      "type": "object",
      "properties": {
        "children": {
          "type": "array",
          "items": [
            {
              "type": "array",
              "items": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            },
            {
              "type": "array",
              "items": [
                {
                  "type": "string"
                },
                {
                  "type": "integer"
                }
              ]
            }
          ]
        },
        "u_salary_1_5_year": {
          "type": "integer"
        }
      },
      "required": [
        "children",
        "u_salary_1_5_year"
      ]
    },
    "name": {
      "type": "string"
    },
    "salary": {
      "type": "integer"
    }
  },
  "required": [
    "age",
    "family",
    "name",
    "salary"
  ]
}

pm.test('Schema is valid', function () {
    pm.expect(tv4.validate(respData, schema)).to.be.true;
});

```

### 3) Проверить что занчение поля name = значению переменной name из окружения
```js
var reqData = JSON.parse(JSON.stringify(pm.request.body.formdata))
var reqName = reqData[2].value
var envName = pm.environment.get("name");

pm.test('Проверить что занчение поля name = значению переменной name из окружения', function() {
    pm.expect(reqName).to.eql(envName);
})
```

### 4) Проверить что значение поля age в ответе соответсвует отправленному в запросе значению поля age
```js
var respData = pm.response.json()
var raspAge = respData.age

var reqData = JSON.parse(JSON.stringify(pm.request.body.formdata))
var reqAge = reqData[0].value

pm.test('Проверить что значение поля age в ответе соответсвует отправленному в запросе значению поля age', function() {
    pm.expect(raspAge).to.eql(reqAge);
});
```
===================

# 6. http://162.55.220.72:5005/currency

POST
auth_token

Resp. Передаётся список массив объектов.
[
{"Cur_Abbreviation": str,
 "Cur_ID": int,
 "Cur_Name": str
}
…
{"Cur_Abbreviation": str,
 "Cur_ID": int,
 "Cur_Name": str
}
]

Тесты:
1) Можете взять любой объект из присланного списка, используйте js random.
В объекте возьмите Cur_ID и передать через окружение в следующий запрос.

 ===================

7) http://162.55.220.72:5005/curr_byn
req.
POST
auth_token
curr_code: int

Resp.
{
    "Cur_Abbreviation": str
    "Cur_ID": int,
    "Cur_Name": str,
    "Cur_OfficialRate": float,
    "Cur_Scale": int,
    "Date": str
}

Тесты:
1) Статус код 200
2) Проверка структуры json в ответе.


===============
***
1) получить список валют
2) итерировать список валют
3) в каждой итерации отправлять запрос на сервер для получения курса каждой валюты
4) если возвращается 500 код, переходим к следующей итреации
5) если получаем 200 код, проверяем response json на наличие поля "Cur_OfficialRate"
6) если поле есть, пишем в консоль инфу про фалюту в виде response
{
    "Cur_Abbreviation": str
    "Cur_ID": int,
    "Cur_Name": str,
    "Cur_OfficialRate": float,
    "Cur_Scale": int,
    "Date": str
}
7) переходим к следующей итерации

