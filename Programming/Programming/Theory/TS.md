# Объединение типов (Union Types) `|`
> [!help]
> **|** - Позволяет создать тип, который может быть одним из нескольких типов.
>
>>[!example]
>>```ts
>>// Тип может быть строкой или числом
>>type StringOrNumber = string | number;
>>
>>function processValue(value: StringOrNumber) {
>>  if (typeof value === 'string') {
>>    return value.toUpperCase();
>>  } else {
>>    return value * 2;
>>  }
>>}
>>```

# Пересечение типов (Intersection Types) `&`
> [!help] 
> **&** - Объединяет несколько типов в один, содержащий все свойства из исходных типов.
>
>>[!example]
>>```ts
>>type Person = {
>>  name: string;
>>  age: number;
>>};
>>
>>type Employee = {
>>  companyId: string;
>>  role: string;
>>};
>>
>>// Содержит все свойства из Person и Employee
>>type EmployeePerson = Person & Employee;
>>
>>const worker: EmployeePerson = {
>>  name: "John",
>>  age: 30,
>>  companyId: "E123",
>>  role: "Developer"
>>};
>>```

# `keyof`
> [!help]
> Оператор **keyof** возвращает объединение всех ключей объектного типа.
>
>>[!example]
>>```ts
>>interface User {
>>  id: number;
>>  name: string;
>>  email: string;
>>}
>>
>>// 'id' | 'name' | 'email'
>>type UserKeys = keyof User;
>>
>>function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
>>  return obj[key];
>>}
>>
>>const user: User = { id: 1, name: "John", email: "john@example.com" >>};
>>const userName = getProperty(user, "name"); // тип: string
>>```


# `in`
> [!help]
> Оператор **in** используется в mapped types для итерации по ключам типа.
>
>>[!example]
>>```ts
>>type Flags = {
>>  option1: boolean;
>>  option2: boolean;
>>};
>>
>>// { readonly option1: boolean; readonly option2: boolean; }
>>type ReadonlyFlags = {
>>  readonly [K in keyof Flags]: Flags[K];
>>};
>>```

# `typeof`
> [!help]
> Оператор **typeof** в контексте типов позволяет получить тип значения.
>
>>[!example]
>>```ts
>>const user = {
>>  id: 1,
>>  name: "John",
>>  roles: ["admin", "editor"]
>>};
>>
>>// Тип: { id: number; name: string; roles: string[] }
>>type UserType = typeof user;
>>
>>function createUser() {
>>  return { id: Date.now(), name: "" };
>>}
>>
>>// Тип возвращаемого значения функции
>>type NewUser = ReturnType<typeof createUser>;
>>```


# Итерация по ключам с помощью `[K in keyof T]`
> [!help]
> Этот паттерн используется для создания mapped types:
>
>>[!example]
>>```ts
>>// Создаем тип, где все свойства исходного типа становятся массивами
>>type ArrayValues<T> = {
>>  [K in keyof T]: T[K][];
>>};
>>
>>interface User {
>>  id: number;
>>  name: string;
>>}
>>
>>// { id: number[]; name: string[]; }
>>type UserArrays = ArrayValues<User>;
>>
>>// Создаем тип с префиксом в именах свойств
>>type Prefixed<T, P extends string> = {
>>  [K in keyof T as `${P}${string & K}`]: T[K];
>>};
>>
>>// { getUserId: number; getUserName: string; }
>>type GetterUser = Prefixed<User, 'getUser'>;
>>```

# `infer` в условных типах
> [!help]
> Позволяет извлекать типы из сложных структур.
>
>>[!example]
>>```ts
>>// Получить тип элементов массива
>>type ElementType<T> = T extends (infer U)[] ? U : never;
>>
>>// string
>>type StringElement = ElementType<string[]>;
>>
>>// Получить типы аргументов функции
>>type FirstArgument<T> = T extends (first: infer U, ...args: any[])=> any ? U : never;
>>
>>function greet(name: string, age: number) {
>>  return `Hello ${name}, you are ${age} years old`;
>>}
>>
>>// string
>>type GreetFirstArg = FirstArgument<typeof greet>;
>>```

# Utility Types
> [!help]
> - `Partial<T>` - Делает все свойства типа ***необязательными***
> - `Required<T>` - Делает все свойства типа ***обязательными***
> - `Readonly<T>` - Делает все свойства ***только для чтения***
> - `Record<K, T>` - Создает тип для обьекта с ***ключами K и значениями типа T***
> - `Pick<T, K>` -  ***Выбирает только*** подмножество свойств K из типа T. **(для обьектов)**
> - `Omit<T, K>` - ***Исключает*** свойства K из типа T. **(для обьектов)**
> - `Exclude<T, U>` ***Исключает*** из T все типы, присутствующие в 
U. **(для перечисления  'circle' | 'square' | 'triangle')**
> - `Extract<T, U>` - ***Извлекает*** из T все типы, присутствующие в U.    **(для перечисления  'circle' | 'square' | 'triangle')**
> - `NonNullable<T>` - ***Исключает `null` и `undefined` из типа T. ***
> - `ReturnType<T>` -  Получает возвращаемый ***тип функции T.***
> - `Parameters<T>` - ***Получает типы параметров функции T*** как кортеж. 
>
>> [!example] `Partial<T>` - Делает все свойства типа необязательными.
>>```ts
>>interface User {
>>  name: string;
>>  age: number;
>>}
>>
>>// { name?: string; age?: number; }
>>type PartialUser = Partial<User>;
>>```
>
>> [!example] `Required<T>` - Делает все свойства типа обязательными.
>>```ts
>>interface Config {
>>  name?: string;
>>  debug?: boolean;
>>}
>>
>>// { name: string; debug: boolean; }
>>type RequiredConfig = Required<Config>;
>>```
>
>> [!example] `Readonly<T>` - Делает все свойства только для чтения.
>>```ts
>>interface Todo {
>>  title: string;
>>  description: string;
>>}
>>
>>// { readonly title: string; readonly description: string; }
>>type ReadonlyTodo = Readonly<Todo>;
>>```
>
>> [!example] `Record<K, T>` -Создает тип с ключами K и значениями типа T. Обычно для обьектов
>>```ts
>>// { home: string; work: string; school: string; }
>>type Locations = Record<'home' | 'work' | 'school', string>;
>>```
>
>> [!example] `Pick<T, K>` Выбирает подмножество свойств K из типа T.
>>```ts
>>interface User {
>>  name: string;
>>  age: number;
>>  email: string;
>>}
>>
>>// { name: string; email: string; }
>>type UserContactInfo = Pick<User, 'name' | 'email'>;
>>```
>
>> [!example] `Omit<T, K>` - Исключает свойства K из типа T.
>>```ts
>>interface User {
>>  name: string;
>>  age: number;
>>  password: string;
>>}
>>
>>// { name: string; age: number; }
>>type PublicUser = Omit<User, 'password'>;
>>```
>
>> [!example] `Exclude<T, U>` - Исключает из T все типы, присутствующие в U.
>>```ts
>> // 'a' | 'b'
>> type ResultType = Exclude<'a' | 'b' | 'c', 'c' | 'd'>;
>>
>> ```
> 
>> [!example] `Extract<T, U>` - Извлекает из T все типы, присутствующие в U.
>>```ts
>> // 'c'
>> type CommonType = Extract<'a' | 'b' | 'c', 'c' | 'd'>;
>> ```
> 
>> [!example] `NonNullable<T>` - Исключает null и undefined из типа T.
>>```ts
>> // string | number
>> type NonNullableType = NonNullable<string | number | null | undefined>;
>> ```
> 
>> [!example] `ReturnType<T>` - Получает возвращаемый тип функции T.
>>```ts
>> function createUser() {
>>  return { name: 'John', age: 25 };
>> }
>> // { name: string; age: number; }
>> type User = ReturnType<typeof createUser>;
>> ```
> 
>> [!example] `Parameters<T>` - Получает типы параметров функции T как кортеж.
>>```ts
>> function fetchData(id: number, filter: string) {
>>   // ...
>> }
>> 
>> // [number, string]
>> type FetchParams = Parameters<typeof fetchData>;
>> ```
> 