# Employee

Сервис Сотрудников (**Employee**) позволяет просматривать, а также централизованно хранить и изменять данные о сотрудниках. 

Это **WebAPI-приложение**, которое взаимодействует с другими сервисами через публичный интерфейс `IEmployeeClient` и REST API.

Таким образом, информация не дублируется, а изменение данных в Employee автоматически отражается во всех организациях сотрудника.

## Хранимые сущности

Используются для СОП и формирования отчетов.

> [!NOTE]
> В Employee сущность привязана к пользователю и является общей для всех его организаций. Пользовательские данные о сущностях изолированы.

Сервис Сотрудников хранит три вида сущностей:

1.  [Person](#person) — `guid` сотрудника и его персональные данные.
2.  [Position](#position) — название должности сотрудника в конкретной организации.
3.  [Role](#role) — роль сотрудника с определенной должностью в конкретной организации.

### Person

Данные хранятся в таблице `employee.persons`.

**Ключ:** `guid` сотрудника.

**Реквизиты:**

*   `guid` — уникальный идентификатор сотрудника;
*   `fullName` — ФИО;
*   `innFl` — ИНН;
*   `birthDate` — дата рождения;
*   `gender` — пол;
*   `citizenship` — гражданство;
*   `birthplace` — место рождения;
*   `phone` — номер телефона;
*   `email` — электронная почта;
*   `registrationAddress` — адрес регистрации;
*   `identityCard` — паспортные данные:
    *   `DocumentType` — тип документа
    *   `Series` — серия
    *   `Number` — номер
    *   `Issuer` — кем выдан
    *   `IssuanceDate` — дата выдачи
    *   `IssuerCode` — код подразделения
*   `foreignAddress` — иностранный адрес.

Подробнее про перечень и формат полей см. [`PersonProps.cs`](https://github.com/NatBur444/task-for-technical-writer/blob/main/Models/PersonProps.cs) и [`Person.cs`](https://github.com/NatBur444/task-for-technical-writer/blob/main/Models/Person.cs).

### Position

Данные хранятся в таблице `employee.positions`.

> [!NOTE]
> Сотрудник может иметь только одну должность в организации.

**Ключ:** `guid` сотрудника + `guid` организации.

**Значение:** `Title` — название должности сотрудника в определенной организации.

Подробнее см. [`Position.cs`](https://github.com/NatBur444/task-for-technical-writer/blob/main/Models/Position.cs).

### Role

Данные хранятся в таблице `employee.roles`.

**Типы ролей:**

1.  `Head` — руководитель;
2.  `ChiefAccountant` — главный бухгалтер;
3.  `Sender` — уполномоченный представитель (отправитель);
4.  `Ip` — индивидуальный предприниматель.

> [!NOTE]
> Сотрудник может относиться к нескольким организациям — иметь в них роль и/или должность. Роль сотрудника в организации не связана с наличием должности в ней. Например, сотрудник может иметь должность Директор в первой организации и роль Sender — во второй.

**Ключ:** `guid` организации + `Type` (тип роли).

**Значение** — документ с двумя полями:

1.  `Person` — `guid` сотрудника с нужной ролью.
2.  `PositionOrganization` — `guid` организации, в которой сотрудник выполняет нужную роль. Не обязательно совпадает с `guid` организации в ключе.

Подробнее см. [`Role.cs`](https://github.com/NatBur444/task-for-technical-writer/blob/main/Models/Role.cs),[`RoleType.cs`](https://github.com/NatBur444/task-for-technical-writer/blob/main/Models/RoleType.cs),[`RoleResponse.cs`](https://github.com/NatBur444/task-for-technical-writer/blob/main/Models/RoleResponse.cs)
