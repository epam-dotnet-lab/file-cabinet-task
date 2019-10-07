# File Cabinet

## Шаг 11 - Расширение команд

Цель: расширение функциональности команд find, list, export.

Все изменения (commits) должны храниться в ветке _step8-extend-commands_, а после окончания работы должны быть слиты в ветку _master_ (использовать squash-merge).


### Задание

1. find - возможность задавать несколько полей через and.

```
> find firstname "John" and lastname "Doe"
#1
#2
```

2. list - возможность задавать поля для отображения.

```
> list id, firstname, lastname
#1, John, Doe
#2, Stan, Smith
```

#### Выборочный экспорт

Добавить возможность выборочно экспортировать данные сервиса по идентификатору записи.

Пример использования:

```sh
> export csv filename.csv 1,2,9-15
Records #1, #2, #9, #10, #11, #12, #13, #14, #15 are exported to file filename.csv.
> export xml filename.xml 1-5,9-15
Records #1, #2, #3, #4, #5, #9, #10, #11, #12, #13, #14, #15 are exported to file filename.xml.
```
