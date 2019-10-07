# File Cabinet

## Шаг 4 - Поиск

Цель: добавление возможностей поиска - команды find.

Все изменения (commits) должны храниться в ветке _step4-add-search_, а после окончания работы должны быть слиты в ветку _master_.


### Выполнение

1. Реализуйте новую команду find, которая должна принимать два параметра - имя свойства и искомый текст. В начале реализуйте команду _find firstname_.

Пример использования:

```sh
> find firstname "Petr"
#1, Petr, Semenov, 1994-Dec-10
#3, Petr, Golovanov, 1989-Ja-01
```

Добавьте в сервис FileCabinetService новый метод FindByFirstName. Добавьте реализацию, которая будет осуществлять поиск.

```cs
public FileCabinetRecord[] FindByFirstName(string firstName) { }
```

Имя свойства и искомый тексты должны быть регистронезависимыми:

```sh
> find FirstName "petr"
#1, Petr, Semenov, 1994-Dec-10
#3, Petr, Golovanov, 1989-Ja-01
> find FIRSTNAME "PETR"
#1, Petr, Semenov, 1994-Dec-10
#3, Petr, Golovanov, 1989-Ja-01
```

Соберите проект, проверьте отсутствие ошибок - *всегда собирайте проект* перед тем как сделать commit. Сделайте commit, сообщение "Add implementation for find firstname command."

2. Расширьте команду find для свойства _lastname_ и сохраните измененения в коммит с названием "Add implementation for find lastname command."

Пример использования:

```sh
> find lastname "Semenov"
#1, Petr, Semenov, 1994-Dec-10
#2, Vasil, Semenov, 1999-Feb-15
> find dateofbirth "1994-Dec-10"
#1, Petr, Semenov, 1994-Dec-10
```

3. Расширьте команду find для свойства _dateofbirth_ и сохраните измененения в коммит с названием "Add implementation for find dateofbirth command.".

```sh
> find dateofbirth "1994-Dec-10"
#1, Petr, Semenov, 1994-Dec-10
```

4. В класс FileCabinetService добавьте поле-словарь, которое должно работать как индекс полю FirstName.

Пример:

```sh
private readonly Dictionary<string, List<FileCabinetRecord>> firstNameDictionary = new Dictionary<string, List<FileCabinetRecord>>();
```

Исправьте методы FileCabinetService.CreateRecord и FileCabinetService.EditRecord таким образом, чтобы словарь обновлялся при добавлении и редактировании записей. Исправьте метод FindByFirstName, чтобы поиск происходил при помощи словаря.

Сохраните изменения в коммит "Improve find firstname performance with dictionary."

5. Добавьте словарь lastNameDictionary и исправьте код, чтобы поиск в FindByLastName происходил при помощи словаря. "Improve find lastname performance with dictionary."

6. Добавьте словарь dateOfBirthDictionary и исправьте код, чтобы поиск в FindByDateOfBirth происходил при помощи словаря. "Improve find dateofbirth performance with dictionary."

7. Сделайте push локальной ветки в удаленную ветку. Затем переключитесь на ветку master и сделайте _squash merge_ изменений из ветки _step4-add-search_.

```sh
$ git push --set-upstream origin step4-add-search
$ git checkout master
$ git merge step4-add-search --squash
```

8. Просмотрите изменения, cделайте push изменений из локальной ветки master в удаленную ветку. Комментарий merge-commit должен содержать комментарии всех commit из целевой ветки.

```sh
$ git status
$ git diff --staged
$ git commit -m "Merge step4-add-search branch: Add implementation for find firstname command. Add implementation for find lastname command. Add implementation for find dateofbirth command. Improve find firstname performance with dictionary. Improve find lastname performance with dictionary. Improve find dateofbirth performance with dictionary."
$ git push
```