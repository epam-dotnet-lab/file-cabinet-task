# File Cabinet

## Описание

## Выполнение

### Шаг 1

Работа с записями, использовать память, List<T>

#### Команда create

```
> create
First name: ...
Last name: ...
Date of birth: ...
Record #1 is created.
```

#### Команда list

```
> list
#1, John, Doe
#2, Stan, Smith
```

#### Команда stat

```
> stat
3 records.
```


### Шаг 2

#### Команда find

```
> find firstname "John"
#1
#2
```

#### Команда edit

```
> edit #1
First name: ...
Last name: ...
Date of birth: ...
```


### Шаг 3 - 

#### Команда export csv

```
> export csv
```


#### Команда export xml

```
> export csv
```

### Шаг 4 -

Добавление файловой системы


### Шаг 5

#### Расширение команды find

```
> find firstname "John", lastname "Doe"
#1
#2
```

#### Расширение команды list

```
> list id, firstname, lastname
#1, John, Doe
#2, Stan, Smith
```

### Шаг 6 -

Добавление in memory таблиц индексов.


### Шаг 7

#### Команда import csv

```
import csv filename.csv
```

### Команда import xml

```
import xml filename.xml
```

### Шаг 8

#### Команда remove

```
> remove #1
Record #1 is removed.
```

#### Команда purge
