# File Cabinet

## Примеры рефакторинга кода

### Пример 1

Исходный код:

```cs
if (o.FileCabinetService.Equals("file", StringComparison.InvariantCultureIgnoreCase))
{
	if (File.Exists(@"cabinet-records.db"))
	{
		this.fileStream = new FileStream(@"cabinet-records.db", FileMode.Open, FileAccess.ReadWrite);
	}
	else
	{
		this.fileStream = new FileStream(@"cabinet-records.db", FileMode.Create, FileAccess.ReadWrite);
	}

	this.fileCabinetService = new FileCabinetFilesystemService(fileStream);
}
else
{
	this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
}
```

1. [Consolidate Duplicate](https://refactoring.guru/ru/consolidate-duplicate-conditional-fragments):

```cs
if (o.FileCabinetService.Equals("file", StringComparison.InvariantCultureIgnoreCase))
{
	var fileMode = File.Exists(@"cabinet-records.db") ? FileMode.Open : FileMode.Create;
	this.fileStream = new FileStream(@"cabinet-records.db", FileMode.Open, FileAccess.ReadWrite);
	this.fileCabinetService = new FileCabinetFilesystemService(fileStream);
}
else
{
	this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
}
```

2. [Extract Method](https://refactoring.guru/ru/extract-method)

```cs
if (o.FileCabinetService.Equals("file", StringComparison.InvariantCultureIgnoreCase))
{
	this.fileStream = CreateFileStream();
	this.fileCabinetService = new FileCabinetFilesystemService(fileStream);
}
else
{
	this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
}

private static FileStream CreateFileStream()
{
	var fileMode = File.Exists(@"cabinet-records.db") ? FileMode.Open : FileMode.Create;
	return new FileStream(@"cabinet-records.db", fileMode, FileAccess.ReadWrite);
}
```

3. [Add parameter](https://refactoring.guru/ru/add-parameter):

```cs
if (o.FileCabinetService.Equals("file", StringComparison.InvariantCultureIgnoreCase))
{
	this.fileStream = CreateFileStream();
	this.fileCabinetService = new FileCabinetFilesystemService(fileStream, @"cabinet-records.db");
}
else
{
	this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
}

private static FileStream CreateFileStream(string dataFilePath)
{
	var fileMode = File.Exists(dataFilePath) ? FileMode.Open : FileMode.Create;
	return new FileStream(dataFilePath, fileMode, FileAccess.ReadWrite);
}
```

4. [Replace Magic Number with Symbolic Constant](https://refactoring.guru/ru/replace-magic-number-with-symbolic-constant)

```cs
const string serviceType = "file";
const string dataFilePath = "cabinet-records.db";

if (o.FileCabinetService.Equals(serviceType, StringComparison.InvariantCultureIgnoreCase))
{
	this.fileStream = CreateFileStream();
	this.fileCabinetService = new FileCabinetFilesystemService(fileStream, dataFilePath);
}
else
{
	this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
}

private static FileStream CreateFileStream(string dataFilePath)
{
	var fileMode = File.Exists(dataFilePath) ? FileMode.Open : FileMode.Create;
	return new FileStream(dataFilePath, fileMode, FileAccess.ReadWrite);
}
```

5. Заменить строковый литерал на enum:

```cs
enum ServiceType
{
	File,
	Memory
}

if (o.FileCabinetService == ServiceType.File)
{
	this.fileStream = CreateFileStream();
	this.fileCabinetService = new FileCabinetFilesystemService(fileStream, dataFilePath);
}
else if (o.FileCabinetService == ServiceType.Memory)
{
	this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
}
else
{
	throw new Exception();
}
```

6. Заменить if...else if...else на switch:

```cs
switch (o.FileCabinetService)
{
	case ServiceType.File:
		this.fileStream = CreateFileStream();
		this.fileCabinetService = new FileCabinetFilesystemService(fileStream, dataFilePath);
		break;
	case ServiceType.Memory:
		this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
	default:
		throw new Exception();
}
```

6. [Extract Method](https://refactoring.guru/ru/extract-method)

```cs
CreateService(o.FileCabinetService);

private static IFileCabinetService CreateService(ServiceType serviceType)
{
	switch (serviceType)
	{
		case ServiceType.File:
			this.fileStream = CreateFileStream();
			this.fileCabinetService = new FileCabinetFilesystemService(fileStream, dataFilePath);
			break;
		case ServiceType.Memory:
			this.fileCabinetService = new FileCabinetMemoryService(recordValidator);
		default:
			throw new Exception();
	}
}
```


### Пример 2

Исходный код:

```cs
foreach (var deserializedRecord in deserializedRecords.Records)
{
    var record = new FileCabinetRecord()
    {
        Id = deserializedRecord.Id,
        FirstName = deserializedRecord.FullName.FirstName,
        LastName = deserializedRecord.FullName.LastName,
        DateOfBirth = DateTime.ParseExact(deserializedRecord.DateOfBirthString, "yyyy-MM-dd", null),
        Gender = deserializedRecord.Gender,
        Salary = deserializedRecord.Salary,
        BonusPoints = deserializedRecord.BonusPoints,
    };

    records.Add(record);
}
```

1. [Extract Method](https://refactoring.guru/ru/extract-method)

```cs
foreach (var deserializedRecord in deserializedRecords.Records)
{
    var record = CreateRecord(deserializedRecord);
    records.Add(record);
}

private static FileCabinetRecord CreateRecord(Record record)
{
	return new FileCabinetRecord()
    {
        Id = deserializedRecord.Id,
        FirstName = deserializedRecord.FullName.FirstName,
        LastName = deserializedRecord.FullName.LastName,
        DateOfBirth = DateTime.ParseExact(deserializedRecord.DateOfBirthString, "yyyy-MM-dd", null)
    };
}
```

2. Linq: Select

```cs
records.AddRange(deserializedRecords.Records.Select(CreateRecord));
```


### Пример 3

Исходный код:

```cs
long position = this.GetFileRecordPosition(reader, fileCabinetRecord.Id) ?? throw new NullReferenceException($"No record with {fileCabinetRecord.Id} id");

private long? GetFileRecordPosition(BinaryReader reader, int recordId)
{
	// ...

    return null;
}
```

Задачей этого метода является последовательный поиск в файле данных записи с указанным ID и возврат позиции в потоке на эту запись. Если запись не найдена, то возвращается null. Данный метод плохо спроектирован, методом пользоваться неудобно и легко совершить ошибку. Использование NullReferenceException совершенно не к месту.

Проблема кода: возвращаемое значение состоит из двух смысловых частей - long содержит позицию, а nullable значение говорит о том, что запись не найдена.

1. Разделение смысловых частей - long? -> bool (true - запись найдета, false - не найдена) + long.

```cs
private bool GetFileRecordPosition(BinaryReader reader, int recordId, out long)
{
	// ...

    return null;
}
```

3. Идиома Try - см. в качестве примера [TryGetValue](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.dictionary-2.trygetvalue)

```cs
private bool TryGetFileRecordPosition(BinaryReader reader, int recordId, out long)
{
	// ...

    return null;
}
```

Теперь метод следует общим правилам BCL. Также стоит добавить новый класс исключения.

```cs
long position;
if (!this.TryGetFileRecordPosition(reader, fileCabinetRecord.Id, out position)) {
    throw new FileRecordNotFound(fileCabinetRecord.Id);
}
```
