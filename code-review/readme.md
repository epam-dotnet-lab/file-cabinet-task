# File Cabinet

## Code Review

### default

Не используйте default для инициализации переменных:

```cs
private static IFileCabinetService fileCabinetService = default;

int id = default;
```

Инициализируйте без значения или используйте литерал:

```cs
private static IFileCabinetService fileCabinetService;

int id; // или
int id = 0;
```

Не используйте default для сравнения вместо null:

```cs
if (fileStream != default)
{
    fileStream.Close();
}
```

Сравнивайте с null:

```cs
if (fileStream != null)
{
    fileStream.Close();
}
```


### Magic numbers and strings

[Применяте константы](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-define-constants) вместо литералов.

Не используйте литерал:

```cs
if (o.Validation.Equals("custom", StringComparison.CurrentCultureIgnoreCase))
```cs

Используйте константу:

```cs
const string customValidationType = "custom";

if (o.Validation.Equals(customValidationType, StringComparison.CurrentCultureIgnoreCase))
```


### TryParse

Используйте TryParse вместо Parse+try...catch: [use Parse if you are sure the value will be valid; otherwise use TryParse.](https://stackoverflow.com/questions/467613/parse-v-tryparse).

```cs
try
{
    id = int.Parse(parameters, CultureInfo.InvariantCulture);
}
catch (FormatException e)
{
    Console.WriteLine(e.Message);
    return;
}
```

Нужно:

```cs
TODO
```


### StringBuilder

Вместо многоразовой конкатенации используйте [StringBuilder](https://docs.microsoft.com/en-us/dotnet/api/system.text.stringbuilder).

```cs
public override string ToString()
{
    return $"{this.Id}, "
    + $"{this.FirstName}, "
    + $"{this.LastName}, "
    + $"{this.DateOfBirth.ToString("yyyy-MMM-dd", CultureInfo.InvariantCulture)}, "
    + $"{this.WorkPlaceNumber}, "
    + $"{this.Salary}, "
    + $"{this.Department}";
}
```

Нужно:

```cs
TODO
```


### ArgumentNullException

Правильно применяйте аргументы [конструктора класса ArgumentNullException](https://docs.microsoft.com/en-us/dotnet/api/system.argumentnullexception.-ctor):

```cs
public FileCabinetRecordCsvReader(StreamReader reader)
{
    if (reader is null)
    {
        throw new ArgumentNullException($"{nameof(reader)} cannot be null.");
    }

    this.reader = reader;
}

```

Нужно:

```cs
public FileCabinetRecordCsvReader(StreamReader reader)
{
    if (reader is null)
    {
        throw new ArgumentNullException(nameof(reader));
    }

    this.reader = reader;
}

```

Если необходимо в исключение передать сообщение - используйте [другой конструктор](https://docs.microsoft.com/en-us/dotnet/api/system.argumentnullexception.-ctor?view=netframework-4.8#System_ArgumentNullException__ctor_System_String_System_String_):

```cs
public FileCabinetRecordCsvReader(StreamReader reader)
{
    if (reader is null)
    {
        throw new ArgumentNullException(nameof(reader), $"{nameof(reader)} cannot be null.");
    }

    this.reader = reader;
}
```


### IDispose

Правильно реализуйте шаблон Disposable - следуйте инструкциям [Implementing a Dispose method](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose).

Неправильно:

```cs
public void Dispose()
{
    this.binaryReader.Dispose();
    this.binaryWriter.Dispose();
}
```


### Кортежи, Tuple, ValueTuple

Аккуратно используйте кортежи в публичных методах классов. Не используйте кортежи в методах, которые будут доступны для других сборок.

1. При использовании Tuple&lt;T&gt; затрудняется чтение кода из-за имен элементов (Item1, Item2, ...).

```cs
public void ValidateParameters(Tuple<string, string, DateTime, short, decimal, char> data)
{
    var firstName = data.Item1;
    var lastName = data.Item2;
}
```

[C# tuple types](https://docs.microsoft.com/en-us/dotnet/csharp/tuples):

_The .NET Framework already has generic Tuple classes. These classes, however, had two major limitations. For one, the Tuple classes named their properties Item1, Item2, and so on. Those names carry no semantic information. Using these Tuple types does not enable communicating the meaning of each of the properties. The new language features enable you to declare and use semantically meaningful names for the elements in a tuple._

2. Кортеж Tuple<T> работает как reference type. Если происходит работа с большим количеством кортежей, то создается большое количесво небольших объектов.

_The Tuple classes cause more performance concerns because they are reference types. Using one of the Tuple types means allocating objects. On hot paths, allocating many small objects can have a measurable impact on your application's performance._

При использовании анонимных кортежей C# 7.0 используется структура [ValueType](https://docs.microsoft.com/en-us/dotnet/api/system.valuetuple-1), которая работает как value type.

Пример кортежа в сигнатуре метода:

```cs
public void ValidateParameters((string fName, string lName, DateTime dob) data);
```

Компилятор преобразовывает такую сигнатуру в вариант с ValueType:

```cs
void ValidateParameters(
    [TupleElementNames(new string[] { "fName", "lName", "dob" })]
    ValueTuple<string, string, DateTime> data);
```

Таким образом, все данные в метод передаются копированием, что аналогично варианту *без кортежа*:

```cs
public void ValidateParameters(string fName, string lName, DateTime dob)
{
    ...
}
```

3. Хрупкость при изменениях. При изменениях в сигнатуре метода ValidateParameters - ломается код классов, которые реализуют _IValidator_, а также код, который вызывает этот метод.

Добавьте еще один параметр и скомпилируйте проект.

```cs
public interface IValidator {
    void ValidateParameters((string fName, string lName, DateTime dob, decimal salary) data);
}
```

Затем сравните с использованием ParameterObject:

```cs
public interface IValidator {
    void ValidateParameters(ValidateParametersData data);
}
```
