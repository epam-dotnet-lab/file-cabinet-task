# File Cabinet

## Описание

Консольное приложение, которое принимает команды пользователя и управляет пользовательскими данными.


## Платформа .NET

Задание использует примеры команд для инструмента командной строки [.NET Core CLI](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet) и ссылается на приложения, которые имеют целевой платформой .NET Core 2.2. Однако задание не имеет привязки к определенной версии платформы .NET, поэтому может быть выполнено с использованием любой версии .NET >= 4.7 и с помощью IDE.

Visual Studio 2019 - это наиболее удобный инструмент для выполнения задания, однако, если конфигурация рабочей машины не позволяет запустить IDE, разработка может производиться с помощью Visual Studio Code и .NET Core SDK.


## Опыт

Требуется:

* Умение работать с командной строкой и утилитой [dotnet](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet). (Альтернатива - навыки работы в Visual Studio).
* Навыки программирования C# - [C# Fundamentals for Absolute Beginners](https://channel9.msdn.com/Series/CSharp-Fundamentals-for-Absolute-Beginners), [C# Tutorials](https://docs.microsoft.com/en-us/dotnet/csharp/tutorials/).
* Навыки работы с [Git](https://git-scm.com/book/ru/v2).
* Английский язык для чтения документации.

Перед началом работы при использовании Visual Studio следует изучить основы отладки в этой IDE - [Debugging .NET Applications](https://github.com/epam-dotnet-lab/tasks/tree/version1/debugging).


## Выполнение

1. [Создание консольного приложения FileCabinetApp](step01.md)
2. [Создание сервиса FileCabinetService](step02.md)
3. [Редактирование и валидация данных](step03.md)
4. [Поиск](step04.md)
5. [Рефакторинг](step05.md)
6. [Экспорт в CSV и XML](step06.md)
7. [Хранилище на файловой системе](step07.md)
8. [Импорт из CSV и XML](step08.md)
9. [Удаление записей](step09.md)
10. [Рефакторинг](step10.md)
11. [Конфигурация и журналирование](step11.md)
12. [Поиск записей в файле](step12.md)
13. [Новые команды управления записями](step13.md)
14. [Улучшенный поиск](step14.md)
15. [Рефакторинг](step15.md)


## Ветвление

В задании используется упрощенная модель ветвления [feature-branch workflow](https://bitworks.software/2018-12-10-git-feature-branch-workflow.html):
* В ветке _master_ всегда находится работоспособный код, который должен компилироваться без ошибок и предупреждений. Все требования текущего шага должны быть выполнены.
* В _feature_ ветке происходит основная работа для текущего шага. Изменения в коммите могут содержать предупреждения, но не должны содержать ошибок. Изменения, которые относятся к одному заданию или являются логически завершенными, заливаются отдельным коммитом.

Таблица имен веток и методов слияния:

| Номер шага | Имя feature-ветки             | Слияние в master |
|------------|-------------------------------|------------------|
| 1          | step1-add-file-cabinet-app    | fast-forward     |
| 2          | step2-add-service             | no fast-forward  |
| 3          | step3-add-validation          | no fast-forward  |
| 4          | step4-add-search              | squash           |
| 5          | step5-refactoring             | squash           |
| 6          | step6-add-export              | squash           |
| 7          | step7-add-filesystem          | squash           |
| 8          | step8-add-import              | no fast-forward  |
| 9          | step9-add-remove              | no fast-forward  |
| 10         | step10-refactoring            | no fast-forward  |
| 11         | step11-add-config-and-logging | squash           |
| 12         | step12-improve-file-find      | squash           |
| 13         | step13-add-select             | squash           |
| 14         | step14-add-cache              | squash           |
| 15         | step15-refactoring            | no fast-forward  |

Более подробно про feature-branch worflow см. в ["Удачная модель ветвления для Git"](https://habr.com/ru/post/106912/).