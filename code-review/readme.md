# File Cabinet

## Code Review

### default

Не используйте default для инициализации переменных:

```cs
private static IFileCabinetService fileCabinetService = default;

int id = default;
```

Нужно

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

Нужно:

```cs
if (fileStream != null)
{
    fileStream.Close();
}
```


### Magic numbers and strings

[Применяте константы](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/how-to-define-constants) вместо литералов:

```cs
if (o.Validation.Equals("custom", StringComparison.CurrentCultureIgnoreCase))
```cs

Нужно:

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

Если необходимо в исключение передать сообщение:

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

Правильно реализуйте шаблон Disposable - [Implementing a Dispose method](https://docs.microsoft.com/en-us/dotnet/standard/garbage-collection/implementing-dispose).

Неправильно:

```cs
public void Dispose()
{
	this.binaryReader.Dispose();
	this.binaryWriter.Dispose();
}
```