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
```

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
 var parsed = int.TryParse(parameters, out int id);

 if (!parsed)
 {
    Console.WriteLine("Invalid id.");
    return;
 }
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
 public override string ToString()
{
    var builder = new StringBuilder();
    builder.Append($"{this.Id}, ");
    builder.Append($"{this.FirstName}, ");
    builder.Append($"{this.LastName}, ");
    builder.Append($"{this.DateOfBirth.ToString("yyyy-MMM-dd", CultureInfo.InvariantCulture)}, ");
    builder.Append($"{this.WorkPlaceNumber}, ");
    builder.Append($"{this.Salary}, ");
    builder.Append($"{this.Department}");
    return builder.ToString();
}
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

2. Кортеж Tuple<T> работает как reference type. Если происходит работа с большим количеством кортежей, то создается большое количество небольших объектов.

_The Tuple classes cause more performance concerns because they are reference types. Using one of the Tuple types means allocating objects. On hot paths, allocating many small objects can have a measurable impact on your application's performance._

При использовании кортежей C# 7.0 используется структура [ValueType](https://docs.microsoft.com/en-us/dotnet/api/system.valuetuple-1), которая работает как value type.

Пример такого кортежа в сигнатуре метода:

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


### Fail Fast

Всегда добавляйте [guard clause](https://deviq.com/guard-clause/), чтобы проверить входные параметры и предотвратить NullReferenceException в неожиданных местах.

Код без guard clause:

```cs
public FileCabinetFileSystemService(FileStream fileStream, IRecordValidator validator)
{
    this.fileStream = fileStream;
    this.validator = validator;
}
```

Код с guard clause:

```cs
public FileCabinetFileSystemService(FileStream fileStream, IRecordValidator validator)
{
    this.fileStream = fileStream ?? throw new ArgumentNullException(nameof(fileStream));
    this.validator = validator ?? throw new ArgumentNullException(nameof(validator));
}
```

См. [практики defensive programming](https://enterprisecraftsmanship.com/posts/defensive-programming/) и принцип [Fail Fast](https://enterprisecraftsmanship.com/posts/fail-fast-principle/).


### Сравнение строк

Вместо:

```cs
record.FirstName.ToUpper(CultureInfo.InvariantCulture) == r.FirstName.ToUpper(CultureInfo.InvariantCulture)
```

Используйте:

```cs
record.FirstName.Equals(r.FirstName, StringComparison.InvariantCultureIgnoreCase)
```


### Extract Variable

Применяйте Extract Variable, если у вас вычисление встречается несколько раз:

```cs
if (!dictionary.ContainsKey(key.ToUpper(CultureInfo.InvariantCulture)))
{
    dictionary.Add(key.ToUpper(CultureInfo.InvariantCulture), new List<FileCabinetRecord>());
}
dictionary[key.ToUpper(CultureInfo.InvariantCulture)].Add(record);
```

Нужно:

```cs
var keyStr = key.ToUpper(CultureInfo.InvariantCulture);
if (!dictionary.ContainsKey(keyStr))
{
    dictionary.Add(keyStr, new List<FileCabinetRecord>());
}
dictionary[keyStr].Add(record);
```

### using

using должен комбинироваться с объявлением переменной. Вместо:

```cs
var csvWriter = new FileCabinetRecordCsvWriter(writer);
using (writer)
{
    ...
}
```

Нужно:

```cs
using (var csvWriter = new FileCabinetRecordCsvWriter(writer))
{
    ...
}
```

С использованием [using declaration](https://csharp.christiannagel.com/2019/04/09/using/):

```cs
using var csvWriter = new FileCabinetRecordCsvWriter(writer);
```

Работа со всем классами, которые реализуют IDispose, должна идти через using. Вместо:

```cs
BinaryReader reader = new BinaryReader(this.fileStream);
```

Нужно:

```cs
using BinaryReader reader = new BinaryReader(this.fileStream);
```


### Environment.NewLine

На разных системах "новая строка" может обозначаться разными символами (или группой символов).

Вместо:

```cs
string.Join("\n", this.errors);
```

Нужно:

```cs
string.Join(Environment.NewLine, this.errors);
```


### Unused namespaces

Удаляйте пространства имен, которые не используются в текущем исходном файле.


### static readonly

Используйте static readonly поля для типов, которые нельзя сделать константами:

```cs
if (DateTime.Compare(DateTime.Now, value) < 0 || DateTime.Compare(new DateTime(1900, 1, 1), value) > 0)
```

Нужно:

```cs
private static readonly DateTime MinDate = new DateTime(1900, 1, 1);

...

if (DateTime.Compare(DateTime.Now, value) < 0 || DateTime.Compare(MinDate, value) > 0)
```


### string.IsNullOrEmpty

Вместо:

```cs
if (firstName.Trim() == string.Empty) { }
```

Нужно:

```cs
if (string.IsNullOrWhiteSpace(firstName)) { }
```
