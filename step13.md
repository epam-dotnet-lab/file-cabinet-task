# File Cabinet

## Шаг 13 - Новые команды управления записями

Цель: реализовать новые команды управления данными (_insert_, _delete_ и _update_).


### Задания

#### Команда insert

Реализовать новую команду _insert_, которая должна добавлять запись, используя переданные данные.

Пример использования:

```
> insert (id, firstname, lastname, dateofbirth) values ('1', 'John', 'Doe', '5/18/1986')
> insert (dateofbirth,lastname,firstname,id) values ('5/18/1986','Stan','Smith','2')
```

#### Команда delete

Реализовать новую команду _delete_, которая должна удалять записи, используя заданные критерии.

Пример использования:

```
> delete where id = '1'
Record #1 is deleted.
> delete where LastName='Smith'
Records #2, #3, #4 are deleted. 
```


#### Команда update

Реализовать новую команду _update_, которая должна обновлять поля записей (кроме id), используя заданные критерии поиска.

Пример использования:

```
> update set firstname = 'John', lastname = 'Doe' , dateofbirth = '5/18/1986' where id = '1'
> update set DateOfBirth = '5/18/1986' where FirstName='Stan' and LastName='Smith'
```


#### Удаление команд

Удалите поддержку команд list, find, edit и remove. Удалите код, который нигде не используется.


#### Подсказка "похожие команды"

Измените код, отвечающий за обработку пользовательских команд, таким образом, чтобы приложение выдавала пользователю подсказку в том случае, если пользователь ошибся при вводе команды. В качестве примера можно посмотреть на работу git:

```sh
$ git sta
git: 'sta' is not a git command. See 'git --help'.

The most similar commands are
        status
        stage
        stash
```

```sh
$ git sowh
git: 'sowh' is not a git command. See 'git --help'.

The most similar command is
        show
```
