# File Cabinet

## Шаг 2 - Создание сервиса FileCabinetService

Цель: создание сервиса (FileCabinetService), который должен хранить записи (FileCabinetRecord) с личной информацией о человеке. Добавление команд create, list и stat.

Все изменения (commits) должны храниться в ветке _step2-add-service_, а после окончания работы должны быть слиты в ветку _master_.


### Материалы

Проработайте дополнительные материалы, чтобы получить недостающие знания и умения.

* Устройство [Array](https://docs.microsoft.com/en-us/dotnet/api/system.array)
* Устройство [List<T>](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.list-1)
* [DateTime.Parse](https://docs.microsoft.com/en-us/dotnet/api/system.datetime.parse)
* [DateTime.TryParse](https://docs.microsoft.com/en-us/dotnet/api/system.datetime.tryparse)
* [CultureInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.cultureinfo)
* [DateTimeFormatInfo](https://docs.microsoft.com/en-us/dotnet/api/system.globalization.datetimeformatinfo)
* [Tuple](https://docs.microsoft.com/en-us/dotnet/api/system.tuple)
* [Action](https://docs.microsoft.com/en-us/dotnet/api/system.action)
* [readonly vs static readonly vs const](https://stackoverflow.com/questions/755685/static-readonly-vs-const)


### Выполнение

1. Создайте новую ветку _step2-add-service_ от ветки _master_.

```sh
$ git branch
* master
  step1-add-file-cabinet-app

$ git checkout -b step2-add-service
Switched to a new branch 'step2-add-service'
```

2. Создайте файл FileCabinetRecord.cs, добавьте в него класс FileCabinetRecord в котором должны быть свойства Id, FirstName, LastName, DateOfBirth.

```cs
public class FileCabinetRecord
{
    public int Id { get; set; }

    public string FirstName { get; set; }

    public string LastName { get; set; }

    public DateTime DateOfBirth { get; set; }
}
```

3. Соберите проект, проверьте отсутствие ошибок, просмотрите изменения. Сделайте commit.

```sh
$ dotnet build
$ git status
$ git add *.cs
$ git diff --staged
$ git commit -m "Add FileCabinetRecord.cs file."
```

4. Создайте файл FileCabinetService.cs, добавьте в него класс FileCabinetService с полем \_list для хранения записей и заглушки методов.

```sh
public class FileCabinetService
{
    private readonly List<FileCabinetRecord> list = new List<FileCabinetRecord>();

    public int CreateRecord(string firstName, string lastName, DateTime dateOfBirth)
    {
        // TODO: добавьте реализацию метода
        return 0;
    }

    public FileCabinetRecord[] GetRecords()
    {
        // TODO: добавьте реализацию метода
        return new FileCabinetRecord[] { };
    }

    public int GetStat()
    {
        // TODO: добавьте реализацию метода
        return 0;
    }
}
```

5. Соберите проект, изучите предупреждения компилятора, однако исправлять код не нужно. Сделайте commit.

```sh
$ dotnet build
Microsoft (R) Build Engine version 16.2.32702+c4012a063 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 30.15 ms for D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj.
FileCabinetService.cs(22,20): warning CA1822: Member GetStat does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(10,20): warning CA1822: Member CreateRecord does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(19,20): warning CA1825: Avoid unnecessary zero-length array allocations.  Use Array.Empty<FileCabinetRecord>() instead. [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(16,36): warning CA1822: Member GetRecords does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(8,50): warning CA1823: Unused field 'list'. [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
  FileCabinetApp -> D:\file-cabinet-task\FileCabinetApp\bin\Debug\netcoreapp2.2\FileCabinetApp.dll

Build succeeded.

FileCabinetService.cs(22,20): warning CA1822: Member GetStat does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(10,20): warning CA1822: Member CreateRecord does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(19,20): warning CA1825: Avoid unnecessary zero-length array allocations.  Use Array.Empty<FileCabinetRecord>() instead. [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(16,36): warning CA1822: Member GetRecords does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(8,50): warning CA1823: Unused field 'list'. [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
    5 Warning(s)
    0 Error(s)

Time Elapsed 00:00:00.96

$ git status
$ git add *.cs
$ git diff --staged
$ git commit -m "Add FileCabinetService.cs file."
```

6. Изучите предупреждение CA1825 и исправьте метод GetRecords, используя подсказку компилятора.

7. Соберите проект. Сделайте commit.

```sh
$ dotnet build
Microsoft (R) Build Engine version 16.2.32702+c4012a063 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 31.71 ms for D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj.
FileCabinetService.cs(22,20): warning CA1822: Member GetStat does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(10,20): warning CA1822: Member CreateRecord does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(16,36): warning CA1822: Member GetRecords does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(8,50): warning CA1823: Unused field 'list'. [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
  FileCabinetApp -> D:\file-cabinet-task\FileCabinetApp\bin\Debug\netcoreapp2.2\FileCabinetApp.dll

Build succeeded.

FileCabinetService.cs(22,20): warning CA1822: Member GetStat does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(10,20): warning CA1822: Member CreateRecord does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(16,36): warning CA1822: Member GetRecords does not access instance data and can be marked as static (Shared in VisualBasic) [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
FileCabinetService.cs(8,50): warning CA1823: Unused field 'list'. [D:\file-cabinet-task\FileCabinetApp\FileCabinetApp.csproj]
    4 Warning(s)
    0 Error(s)

Time Elapsed 00:00:02.03

$ git status
$ git add *.cs
$ git diff --staged
$ git commit -m "Fix unnecessary zero-length array allocation in FileCabinetService.cs."
```

8. Добавьте в файл Program.cs новый метод Stat, который должен выводить статистику по записям. Добавьте необходимый код, чтобы метод вызывался, когда пользователь вводит команду _stat_. Добавьте поддержку справки.

```cs
private static void Stat(string parameters)
{
    var recordsCount = Program.fileCabinetService.GetStat();
    Console.WriteLine($"{recordsCount} record(s).");
}
```

9. Добавьте реализацию метода FileCabinetService.GetStat, чтобы метод возвращал количество записей, которые хранит сервис.

```cs
public int GetStat()
{
    return this.list.Count;
}
```

10. Соберите проект. Сделайте commit.

```sh
$ dotnet build
$ git status
$ git add *.cs
$ git diff --staged
$ git commit -m "Add implementation for stat command."
```

11. Добавьте реализацию метода FileCabinetService.CreateRecord, который должен сохранять данные в запись и возвращать идентификатор новой записи.

```cs
public int CreateRecord(string firstName, string lastName, DateTime dateOfBirth)
{
    var record = new FileCabinetRecord
    {
        Id = this.list.Count + 1,
        FirstName = firstName,
        LastName = lastName,
        DateOfBirth = dateOfBirth,
    };

    this.list.Add(record);

    return record.Id;
}
```

12. Добавьте реализацию метода Program.Create, который должен получать пользовательский ввод и вызывать метод FileCabinetService.CreateRecord с данными, которые ввел пользователь. Добавьте поддержку команды create, которая должна вызывать метод Program.Create.

```
> create
First name: Petr
Last name: Semenov
Date of birth: 12/10/1994
Record #1 is created.
```

Формат даты - месяц/день/год.

13. Соберите проект. Сделайте commit.

```sh
$ dotnet build
$ git status
$ git add *.cs
$ git diff --staged
$ git commit -m "Add implementation for create command."
```

14. Добавьте реализацию метода FileCabinetService.GetRecords, который должен возвращать копию внутреннего списка сервиса.

15. Реализуйте новый метод Program.List и добавьте поддержку новой команду _list_, которая должна возвращать список записей, добавленных в сервис.

```
> list
#1, Petr, Semenov, 1994-Dec-10
```

Формат даты - год-месяц-день.


16. Соберите проект. Если компилятор выводит сообщения об ошибках или предупреждения, исправьте код. Сделайте commit.

```sh
$ dotnet build
$ git status
$ git add *.cs
$ git diff --staged
$ git commit -m "Add implementation for list command."
```

17. Добавьте в класс FileCabinetRecord новые свойства - минимум 3 с типами short, decimal, char. Исправьте код сервиса и обработчиков команд create и list, чтобы пользователь мог видеть и добавлять данные для этих полей.

18. Соберите проект. Если компилятор выводит сообщения об ошибках или предупреждения, исправьте код. Сделайте commit - добавьте имена свойств в текст сообщения.

```sh
$ dotnet build
$ git status
$ git add *.cs
$ git diff --staged
$ git commit -m "Extend FileCabinetRecord class with new properties - <property1>, <property2>, <property2>."
```

19. Сделайте push локальной ветки в удаленную ветку.

```sh
$ git push --set-upstream origin step2-add-service
```

20. Переключитесь на ветку master и сделайте no fast-forward merge изменений из ветки _step2-add-service_.

```sh
$ git checkout master
$ git merge step2-add-service --no-ff
```

21. Сделайте push изменений из локальной ветки master в удаленную ветку.

```sh
$ git push
```

22. Посмотрите журнал коммитов текущей ветки (_master_) и найдите merge commit (коммит с описанием "Merge branch 'step2-add-service'") - он появился, так как при слиянии был применен ключ --no-ff.

```sh
$ git log --oneline
62948a6 (HEAD -> master, origin/master, origin/HEAD) Merge branch 'step2-add-service'
e8e282a (origin/step2-add-service, step2-add-service) Extend FileCabinetRecord class with new properties - Property1, Property2, Property3.
8fcebaf Add implementation for list command.
7175748 Add implementation for create command.
e4365ac Add implementation for stat command.
d08f69f Fix unnecessary zero-length array allocation in FileCabinetService.cs.
11c9306 Add FileCabinetService.cs file.
05bc938 Add FileCabinetRecord.cs file.
5e24f45 (origin/step1-add-file-cabinet-app, step1-add-file-cabinet-app) Change DeveloperName to Dmitry Georgiev.
93f9f64 Add FileCabinetApp skeleton.
97e9145 Add initial version of FileCabinetApp.
227f3b7 Initial commit
```


### Проверка

* Проверьте работоспособность приложения - команд create, list и stat.


### Проверочные вопросы

* Для чего нужен Tuple?
* Для чего нужен Action?
* Какая разница между массивом и List<T>?
* Как устроен List<T>?
* Для чего нужен CultureInfo?
* Какая разница между readonly, static readonly и const?
