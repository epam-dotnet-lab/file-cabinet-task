# File Cabinet

## Шаг 10 - Рефакторинг

Цель: улучшение структуры кодовой базы и качества кода.

Все изменения должны храниться в ветке _step10-refactoring_, а после окончания работы должны быть слиты в ветку _master_ с помощью *no fast-forward* merge.

Изменения каждого шага (нумерованного абзаца) должны находиться в отдельном коммите, который должен иметь понятное и ясное описание.

### Задание

#### Рефакторинг команд

Обработчики команд хранятся в статическом массиве _commands_, а код обработчиков - в статических методах класса _Program_. Поддерживать этот код при большом количестве команд приложения не удобно.

1. Примените шаблон [Chain of Responsibility](https://refactoring.guru/ru/design-patterns/chain-of-responsibility):

![Getting started with chain of Responsibility](images/step10-chain-of-responsibility-start.png)

* Создайте интерфейсы и классы, как показано на диаграмме классов, в новом каталоге _CommandHandlers_.
* Перенесите в класс _CommandHandler_ код (методы и поля) всех обработчиков.
* Сделайте поля _fileCabinetService_ и _isRunning_ публичными, чтобы обработчики могли работать с сервисом и останавливать приложение.
* Создайте новый private static метод _Program.CreateCommandHandlers_, который должен создавать и возвращать объект _CommandHandler_.
* Метод _Program.Main_ должен получать обработчик от _CreateCommandHandlers_ и вызывать метод _ICommandHandler.Handle_.

2. Примените технику [Extract Class](https://refactoring.guru/ru/extract-class) и выделите поведение всех обработчиков в отдельные классы.

![Chain of Responsibility](images/step10-chain-of-responsibility.png)

Исправьте метод _CreateCommandHandlers_, чтобы он создавал все обработчики.

3. Открытое поле _fileCabinetService_ - это нарушение инкапсуляции. Сделайте это поле закрытым и примените Constructor Injection к классам-обработчикам, которые используют сервис:

![Chain of Responsibility with Constructor Injection](images/step10-chain-of-responsibility-di.png)

4. У некоторых классов появилась общая структура - примените технику [Extract Superclass](https://refactoring.guru/ru/extract-superclass):

![Chain of Responsibility](images/step10-chain-of-responsibility-service.png)

5. Закройте поле _isRunning_, сделайте Constructor Injection делегата _Action\<bool>_ в класс _ExitCommandHandler_. Используйте делегат, чтобы изменять значение поля _isRunning_.


#### Поддержка изменений

Команды "list" и "find" выводят на экран список записей в одном стиле, следовательно должны иметь общий код для отображения списка. Предположим, вы точно знаете, что в будущем потребуется поддерживать другой стиль отображения списка. Значит, функцию печати списка можно рассмотреть как "причину изменений" для классов, которые занимаются обработкой команд.

1. Примените Strategy Pattern к классам _ListCommandHandler_ и _FindCommandHandler_ как показано на диаграмме классов:

![Strategy Pattern for printing a record list](images/step10-printer-strategy.png)

2. Вместо классов возможно использовать делегаты - [такой подход тоже будет стратегией](https://stackoverflow.com/questions/529524/in-c-sharp-what-is-the-difference-between-strategy-pattern-and-delegates).

* Замените _IRecordPrinter_ на _Action<IEnumerable\<FileCabinetRecord>>_ в классах-обработчиках.
* Метод _DefaultRecordPrinter.Print_ перенесите в метод _Program.DefaultRecordPrint_.
* В методе _Program.CreateCommandHandlers_ передавайте в конструктор метод  _Program.DefaultRecordPrint_ как делегат.
* В классах-обработчиках вызывайте делегат для печати списка записей.


#### 




#### Качество кода

1. 

1. Проверьте качество кода и скорректируйте код, используя руководство из шага 5.

