# REST API Style Guide

## Введение

При проектировании и разработке HTTP REST API, консистентность и последовательность в именовании параметров и ресурсов является важнейшим аспектом, который влияет на понятность и удобство использования, а также интеграций с вашим API.

Консистентность (или согласованность) означает использование одинаковых, похожих и понятных (прозрачных) обозначений для свойств, методов и других элементов.

Единый подход позволяет снизить когнитивную нагрузку как на разработчика, так и на конечного пользователя:

* Проектирование
  
  Нет необходимости каждый раз изобретать названия, сопостовлять параметры и свойства друг с другом при интеграциях. Это позволит снизить сложность, увеличит скорость проектирования решений.

* Чтение
  
  Стандарт позволит ускорить погружение и прозрачность при изучении API, снизит количество недопониманий или лишних шагов в использовании API, облегчит интеграции с уже существующими решениями.

* Реализация

  Стандарт позволит снизить вероятность возможности опечаток, т.к. IDE сможет выбирать из уже существующих переменных, позволит переиспользовать компоненты, снизит время на code-review.

## Свойства

Существуют разные практики оформления свойств:

* camelCase, например "createdAt": 1320296464

* snake_case, например "created_at": "Thu Nov 03 05:19;38 +0000 2011"

* PascalCase, например "CreatedAt": "2011-10-29T09:35:00Z"

* kebab-case, например "created-at": "2011-10-29T09:35:00Z"

* UPPER_CASE_SNAKE_CASE, например "CREATED_AT": "2011-10-29T09:35:00Z"

и другие, менее популярные варианты.

Мы используем **camelCase**.

во всех официальных туториалах javascript переменные в основном называются через camelCase, поэтому при работе с JSON (javascript нотация ведь), придерживаюсь такого стиля

На мой взгляд, это наиболее приятный для чтения и часто встречающийся в API вариант. Хотя здесь вопрос скорее предпочтений.

## Нейминг

### Множетсвенные и единственные числа

Названия должны быть согласованы в множественных и единственных числах.

Например:

Идентификатор пользователя назван как userId, тогда список идентификаторов должен быть назван userIds.
Аналогично, с кодами ошибок: если ошибка - это errorCode, то список ошибок - errorCodes.

### Имя должно отражать суть своего содержимого

По названию поля должно быть понятно, что в нем содержится. Старайтесь делать так, чтобы ваши поля говорили сами за себя.

Например, если поле названо как как userList, то это должен быть список объектов User-ов.

Как сказал один из системных аналитиков команды: "Выглядит очень загадочно".
Это не может быть список id пользователей.

Если без технической документации вам не понятно, что нужно вписать в поле, или что конкретно вы получите в ответе - стоит его поменять.

Еще один пример:

```text
[{
  "request": "N12345",
  "author": "Anna"
}]
```

По названию поля author невозможно сразу, без документации или получения ответа, понять что в ответе будет именно имя.

Правильнее будет передавать объект, который описывает автора: со свойствами id, name и так далее.

Если же необходимо передать именно имя, то лучше так и обозначить: authorName.

Старайтесь избегать именований, из которых не ясно их влияние или действие на сервис.

### Старайтесь, чтобы названия были согласованы между слоями приложения

Старайтесь иметь согласованность в названиях полей между слоями.

Например, если в таблице у вас есть поле created_at и это поле необходимо отдавать в API, то логично в слое API назвать это поле createdAt.

Без необходимости не стоит ломать нейминг полей в разных слоях.

Это упростит жизнь и аналитику, и тестировщику, и разработчику, и команде поддержки.

### Отсутствует полиморфизм в структуре

Вернемся к одному из предыдущих примеров. Например, содержимое ответа на запрос `GET /requests` выглядит так:

```text
{
  "request": "1",
  "author": "Anna"
}
```

А ответ на запрос `GET /requests/1` так:

```text
{
  "request": "1",
  "author": {
    "id": "123",
    "name": "Anna"
  }
}
```

Это полиморфизм структуры ресурса (в данном случае вложенного):

* В одном случае поле author  — это строка с именем

* В другом случае поле author — это объект с вложенными данными

Таких ситуаций лучше не допускать.

Понятно, что иногда так делают, чтобы просто не возвращать все данные и укоротить ответ. Но в таком случае стоит завести отдельное поле или объект под 'укороченный' ответ.

Чем структурированнее будет ответ, тем проще с ним будет работать.

## Правила наименования

Если не придерживаться справочника, то в одном ответе будет свойство pctPrice, в другом pricePct, рядом еще createdDate с createdAt и будет сложно как проектировать, так и разрабатывать, не говоря уже об тестировании.

Для стандартизации ниже приведены общие правила.

### Пагинация

TODO

limit, offset (Facebook)

page, rpp (records per page) (Twitter)

pageNumber, pageSize

start, count (LinkedIn)

### Сортировка

TODO

Сортировка

sort, sortBy, sortProperty - признак сортировки

order, sortDirection - направление сортировки

### Дата и время

Для работы с датой и временем используется постфикс smthAt:

```text
createdAt
executedAt
```

### Проценты

Если необходимо явно указать, что работа идет с процентами, используйте постфикс smthPct:

```text
discountPct
```

### Текстовый поиск

TODO

q, subString, search, text

### Диапазоны

TODO

startDate, endDate

startAt, stopAt

depart_start, depart_range (количество дней +- от даты)(aviasales)

price_min, price_max
