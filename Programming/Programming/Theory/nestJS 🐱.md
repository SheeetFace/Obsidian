# NestJS
> [!help] 
>**NestJS** - это современный фреймворк для создания масштабируемых серверных приложений на Node.js, построенный на TS и вдохновлённый архитектурой Angular.
>***Главная цель*** — обеспечить чистоту, модульность и лёгкость масштабирования кода, используя лучшие практики объектно-ориентированного, функционального и реактивного программирования.

# Архитектура-NestJS
> [!help] 
> Ключевые архитектурные элементы:
>   1. **Модули**
>       - базовая единица архитектуры NestJS. Каждый модуль инкапсулирует определённую функциональность, объединяя `контроллеры`, `сервисы`, `провайдеры` и другие модули. Это позволяет разбивать приложение на независимые, переиспользуемые части, что облегчает поддержку и масштабирование
>>[!example]
>>```ts
>>import { Module } from '@nestjs/common';
>>import { UsersController } from './users.controller';
>>import { UsersService } from './users.service';
>>
>>@Module({
>>  controllers: [UsersController],
>>  providers: [UsersService],
>>})
>>export class UsersModule {}
>>```
>
>   2. **Контроллеры**
>       - отвечают за обработку входящих `HTTP-запросов` и маршрутизацию их к соответствующим сервисам. Каждый контроллер реализует обработчики маршрутов (например, `GET`, `POST`), которые вызываются при получении запроса определённого типа и `URL`
>>[!example]
>>```ts
>>import { Controller, Get } from '@nestjs/common';
>>import { UsersService } from './users.service';
>>
>>@Controller('users') //url по типу www.example.com/users
>>export class UsersController {
>>  constructor(private readonly usersService: UsersService) {}
>>
>>  @Get()
>>  findAll() {
>>    return this.usersService.findAll();
>>  }
>>}
>>```
>
>   3. **Сервисы (провайдеры)**
>       - содержат бизнес-логику приложения и предоставляют методы для выполнения операций. Они реализуются как классы с декоратором `@Injectable()` и могут быть внедрены в контроллеры или другие сервисы через механизм инъекции зависимостей (Dependency Injection)
>>[!example]
>>```ts
>>import { Injectable } from '@nestjs/common';
>>
>>@Injectable()
>>export class UsersService {
>>  private readonly users = [];
>>  findAll() {
>>    return this.users;
>>  }
>>}
>>```
>
>###### Принципы архитектуры
>   1. ***Модульность***
>       - Приложение делится на небольшие независимые модули, что облегчает организацию кода, его переиспользование и масштабирование.
>   2. ***Инъекция зависимостей*** (Dependency Injection, DI)
>       - NestJS активно использует DI-контейнер для управления зависимостями между компонентами. Это снижает связанность кода, упрощает тестирование и замену компонентов.
>   3. ***Декораторы***
>       - Классы и методы помечаются декораторами (`@Module`, `@Controller`, `@Injectable`, `@Get`и др.), которые предоставляют фреймворку метаданные для организации структуры приложения и маршрутизации.
>
>###### Структура типового проекта
>    - `src/main.ts` - точка входа, инициализация приложения.
>    - `src/app.module.ts` - корневой модуль, объединяющий остальные модули, контроллеры и сервисы.
>    - `src/app.controller.ts` - роуты.
>    - `src/app.service.ts` - логика.
>
>###### Преимущества архитектуры NestJS
>    - Строгая типизация и поддержка TS.
>    - Чёткая структура и модульность.
>    - Лёгкая масштабируемость и повторное использование кода.
>    - Удобная система внедрения зависимостей.
>    - Гибкость для интеграции с разными базами данных, шаблонизаторами и сторонними библиотеками.
>

# декораторы-для-HTTP-запросов
>[!help]
>   - `@Get()` - обрабатывает HTTP GET-запросы.
>   - `@Post()` - обрабатывает HTTP POST-запросы.
>   - `@Put()` - обрабатывает HTTP PUT-запросы.
>   - `@Delete()` - обрабатывает HTTP DELETE-запросы.
>   - `@Patch()` - обрабатывает HTTP PATCH-запросы.
>   - `@Options()` - обрабатывает HTTP OPTIONS-запросы.
>   - `@Head()` - обрабатывает HTTP HEAD-запросы.
>   - `@All()` - обрабатывает все типы HTTP-запросов для указанного маршрута.

# декораторы для извлечения данных
>[!help]
>   - `@Param()` - для получения параметров маршрута.
>>[!example] 
>>```ts 
>>@Get('user/:id') 
>>getUserById(@Param('id') id: string) {
>>  return `User ID: ${id}`;
>>}
>>// URL: GET /user/42 -> "User ID: 42"
>>```
>   - `@Query()` - для получения query-параметров.
>>[!example]
>>```ts 
>>@Get('search')
>>searchUsers(@Query('name') name: string) {
>>  return `Searching for: ${name}`;
>>}
>>// URL: GET /search?name=John -> "Searching for: John"
>>```
>   - `@Body()` - для получения тела запроса.
>>[!example]
>>```ts 
>>@Post('create-user')
>>createUser(@Body() body: { name: string }) {
>>  return `Created user: ${body.name}`;
>>}
>>// URL: POST /create-user { "name": "Jane" } -> "Created user: Jane"
>>```
>   - `@Req()` или `@Request()` - для доступа к объекту запроса.
>>[!example]
>>```ts 
>>@Get('request')
>>getRequestInfo(@Req() req: Request) {
>>  return `Request method: ${req.method}`;
>>}
>>// URL: GET /request -> "Request method: GET"
>>```
>   - `@Res()` или `@Response()` - для доступа к объекту ответа.
>>[!example]
>>```ts 
>>@Get('custom-response')
>>sendCustomResponse(@Res() res: Response) {
>>  res.status(201).send('Custom response sent!');
>>}
>>// URL: GET /custom-response -> HTTP Status 201 with "Custom response sent!"
>>```
>   - `@Headers()` - для получения заголовков запроса.
>>[!example]
>>```ts 
>>@Get('headers')
>>getHeaders(@Headers('user-agent') userAgent: string) {
>>  return `User-Agent: ${userAgent}`;
>>}
>>// URL: GET /headers (с указанием User-Agent) -> "User-Agent: <ваш агент>"
>>```
>   - `@Ip()` - для получения IP-адреса клиента.
>>[!example]
>>```ts 
>>@Get('client-ip')
>>getClientIp(@Ip() ip: string) {
>>  return `Client IP: ${ip}`;
>>}
>>// URL: GET /client-ip -> "Client IP: 192.168.1.1"
>>```
>   - `@Session()` - для работы с сессией.
>>[!example]
>>```ts 
>>@Get('session')
>>getSessionData(@Session() session: Record<string, any>) {
>>  return session;
>>}
>>// URL: GET /session (с активной сессией) -> данные сессии
>>```

# DTO
>[!help]
>**DTO** (Data Transfer Object) - объект для определения структуры данных, которые принимаются или отправляются, помогает валидации и типизации.

# middleware-guards-pipes-interceptors
>[!help]
>В NestJS есть четыре важных механизма для обработки запросов: 
> - **middleware** 
> - **guards** 
> - **pipes** 
> - **interceptors** 
>
>Они выполняют разные роли и располагаются в определённом порядке в жизненном цикле запроса.
>
>   1. **Middleware** (Промежуточное ПО)
>       - ***Что делает***: Выполняет обработку запроса ***до попадания в контроллер***. Обычно используется для логирования, парсинга, аутентификации, изменения запроса.
>       - ***Где подключать***: В модулях через метод `configure()` класса модуля с помощью `consumer.apply(MiddlewareClass).forRoutes(...)`.
>>[!example]
>>```ts
>>import { Injectable, NestMiddleware } from '@nestjs/common';
>>import { Request, Response, NextFunction } from 'express';
>>
>>@Injectable()
>>export class LoggerMiddleware implements NestMiddleware {
>>  use(req: Request, res: Response, next: NextFunction) {
>>    console.log(`Request... ${req.method} ${req.url}`);
>>    next();
>>  }
>>}
>>
>>import { Module, MiddlewareConsumer } from '@nestjs/common';
>>
>>@Module({})
>>export class AppModule {
>>  configure(consumer: MiddlewareConsumer) {
>>    consumer.apply(LoggerMiddleware).forRoutes('*'); // подключаем глобально ко всем маршрутам
>>  }
>>}
>>```
>   2. **Guards (Защитники)**
>       - ***Что делает***:Отвечают за проверку доступа (авторизацию, аутентификацию). Выполняются после `middleware`, но до контроллера.
>       - ***Где подключать***: Через декоратор `@UseGuards()` на уровне контроллера или метода, либо глобально через `app.useGlobalGuards()`.
>>[!example]
>>```ts
>>import { Injectable, CanActivate, ExecutionContext } from '@nestjs/common';
>>
>>@Injectable()
>>export class AuthGuard implements CanActivate {
>>  canActivate(context: ExecutionContext): boolean {
>>    const request = context.switchToHttp().getRequest();
>>    return Boolean(request.headers.authorization); // простая проверка наличия заголовка
>>  }
>>}
>>
>>import { Controller, Get, UseGuards } from '@nestjs/common';
>>
>>@Controller('cats')
>>@UseGuards(AuthGuard) // применяем ко всем методам контроллера
>>export class CatsController {
>>  @Get()
>>  findAll() {
>>    return ['cat1', 'cat2'];
>>  }
>>}
>>```
>   3. ** Pipes (Трубы)**
>       - ***Что делает***: Преобразуют и валидируют данные входящего запроса (например, парсят строки в числа, проверяют DTO).
>        - ***Где подключать***: Через декоратор `@UsePipes()` на уровне контроллера, метода или параметра, либо глобально через `app.useGlobalPipes()`.
>>[!example]
>>```ts
>>import { PipeTransform, Injectable, ArgumentMetadata, BadRequestException } from '@nestjs/common';
>>
>>@Injectable()
>>export class ParseIntPipe implements PipeTransform<string, number> {
>>  transform(value: string, metadata: ArgumentMetadata): number {
>>    const val = parseInt(value, 10);
>>    if (isNaN(val)) {
>>      throw new BadRequestException('Validation failed');
>>    }
>>    return val;
>>  }
>>}
>>
>>import { Controller, Get, Param, UsePipes } from '@nestjs/common';
>>
>>@Controller('items')
>>export class ItemsController {
>>  @Get(':id')
>>  @UsePipes(ParseIntPipe)
>>  findOne(@Param('id') id: number) {
>>    return `Item #${id}`;
>>  }
>>}
>>```
>   4. **Interceptors (Перехватчики)**
>       - ***Что делает***:Перехватывают вызовы методов контроллера, могут изменять входящие данные, результат, обрабатывать ошибки, логировать время выполнения.
>       - ***Где подключать***: Через декоратор `@UseInterceptors()` на уровне контроллера, метода или глобально через `app.useGlobalInterceptors()`.
>>[!example]
>>```ts
>>import { Injectable, NestInterceptor, ExecutionContext, CallHandler } from '@nestjs/common';
>>import { Observable } from 'rxjs';
>>import { tap } from 'rxjs/operators';
>>
>>@Injectable()
>>export class LoggingInterceptor implements NestInterceptor {
>>  intercept(context: ExecutionContext, next: CallHandler): Observable<any> {
>>    console.log('Before...');
>>    const now = Date.now();
>>    return next.handle().pipe(
>>      tap(() => console.log(`After... ${Date.now() - now}ms`)),
>>   );
>>  }
>>}
>>
>>import { Controller, Get, UseInterceptors } from '@nestjs/common';
>>
>>@Controller('cats')
>>@UseInterceptors(LoggingInterceptor)
>>export class CatsController {
>>  @Get()
>>  findAll() {
>>    return ['cat1', 'cat2'];
>>  }
>>}
>>```


# типы-модулей
>[!help]
>| Тип модуля       | Описание                                              | Пример                    |
|------------------|-------------------------------------------------------|---------------------------|
| Корневой         | Точка входа, объединяет всё приложение                | `AppModule`                 |
| Функциональный   | Бизнес-логика определённой области                    | `UsersModule`               |
| Ядра             | Базовые сервисы и инфраструктура                      | `DatabaseModule`            |
| Интеграционный   | Связь между разными модулями и внешними сервисами     | `NotificationsModule`       |
| Системный        | Запуск и поддержка работы приложения                  | `MainSystemModule`          |
| Динамический     | Позволяет конфигурировать модуль при импорте          | `DatabaseModule.forRoot`    |
| Глобальный       | Провайдеры доступны во всём приложении                | `GlobalConfigModule`        |

