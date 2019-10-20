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