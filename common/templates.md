# Обзор языка шаблонов Movable Type

Система шаблонов в Movable Type устроена таким образом, что вы можете отобразить контент в точности, как пожелаете. Шаблоны позволяют указать, где будет расположен контент и как он будет выглядеть. Для этого используются специальные теги, которые обычно добавляют к другой разметке, например, к HTML или XML.

Когда создается новый блог, то в нём автоматически создаются стандартные шаблоны. Администратор блога или любой другой пользователь, у которого есть права на редактирование шаблонов, может просматривать или редактировать эти шаблоны.

Большая часть содержания шаблона состоит из неизменяемых частей, например, HTML-тегов (или XML для фидов) и обычного текста. Области, в которых будет постоянно обновляющийся контент, заключены в теги шаблонов Movable Type. Эти теги очень похожи на обычные HTML или XML теги, так что дизайнерам не составит большого труда понять особенности работы с ними и не нужно будет изучать какие-либо языки программирования, чтобы просто изменить внешний вид записей в блоге.

## Синтаксис тегов шаблонов

Для тех, кто знаком с HTML и XML, тега шаблонов Movable Type покажутся вполне знакомыми. Все теги шаблонов заключены в угловые скобки. Существуют блоковые теги и строчные. Блоковые состоят из пары тегов — открывающего и закрывающего. А строчные — из одного.

### Примеры тегов

`<mt:EntryTitle/>` — строчный тег.

`<mt:Entries>…</mt:Entries>` — блоковый тег.

_Обратите внимение:_ вам могут встретиться различные варианты оформления тегов Movable Type, но _рекомендуется использовать стиль XML_, чтобы избежать конфликтов в текстовых редакторах и обеспечить совместимость с будущими версиями. Например, тег `<MTEntries>` равозначен тегу `<mt:Entries>`.

`<mt:Entries>` и `<MTEntries>` — равнозначные теги.

`<mt:EntryTitle/>` и `<$MTEntryTitle$>` — равнознычные теги.

В тегах шаблонов используется префикс, известный также как пространство имен, чтобы теги Movable Type можно было легко отличить от HTML-тегов или любых других языков разметки. Кроме того, теги шаблонов можно использовать внутри других языков: PHP, ASP, JSP, и т.д.

```xml
<mt:Entries>
    <mt:EntryTitle/>
</mt:Entries>
```

Теги шаблонов не чувствительны к регистру, поэтому можно выбирать любой удобный вид их отображения (хотя для наилучшей читаемости рекомендуется использовать первый вариант из примера). Например, следующие теги являются эквивалентными:

```xml
<mt:FooBar>
<mt:foobar>
<MT:fooBar>
```

### Атрибуты тегов шаблонов

Большинство тегов поддерживает атрибуты, которые позволяют изменить стандартное поведение тега. Атрибуты тегов — это ещё одна особенность тегов шаблонов, которая покажется знакомой тем, кто уже работал с HTML или XML. Атрибут — это пара — ключ и его значение, которая находится в имени тега и отделяется от него пробелом. Значение атрибута всегда должно быть заключено в двойные или одинарные кавычки. Пример:

```xml
<mt:Entries lastn="40">
    <mt:EntryTitle/>
</mt:Entries>
```

В этом примере `lastn` является одним из атрибутов блокового тега `<mt:Entries>` и имеет значение 40. В результате выполнения этого кода будет отображено 40 заголовков последних записей. Большинство тегов имеют необязательные атрибуты, но в некоторых тегах существуют атрибуты, которые нужно обязательно указывать.

Кроме того, в Movable Type существует набор [[набор глобальных фильтров|filters]], которые могут применяться ко всем тегам.

## Типы тегов шаблонов

В Movable Type существуют три типа тегов шаблонов: переменные, блоковые и условные.

### Переменные теги (теги-переменные)

Переменный тег просто выводит значение для указанного тега. Например, это может быть название блога или заголовок записи. Пример:

```xml
<mt:BlogName/>
<mt:EntryTitle/>
```

### Блоковые теги

Блоковые теги (иначе — контейнерные теги) получили своё название из-за того, что содержат внутри себя другие теги или какой-либо текст. А также состоят из открывающего и закрывающего тега.

Блоковые теги обычно используются для вывода списка элементов, оформление которого будет задано внутри блокового тега. То есть они как бы объявляют: «вот здесь будет список записей, а как он будет оформлен — смотри моё содержимое». Пример:

```xml
<mt:Entries>
Запись «<mt:EntryTitle/>» написана <mt:EntryAuthor/> в <mt:EntryDate/>
</mt:Entries>
```

В этом примере `<mt:Entries>` — это блоковый тег, выводящий список записей. Каждая отображённая запись будет содержать заголовок, заключённый в кавычки, автора записи и дату, когда запись была опубликована.

### Условные теги

Условные теги — это особая форма блоковых тегов, которая, прежде чем вывести содержимое, проверяет указанные условия. Если условие удовлетворено, то дальнейшие теги будут обработаны.
 
```xml
<mt:If tag="EntryCommentCount" ge="1">
У этой записи больше одного комментария
</mt:If>
```

В этом примере проверяется количество комментариев к записи. И, если оно больше или равно 1, то выводится текст «У этой записи больше одного комментария».

Условные теги легко узнать, так как они всегда начинаются с `<mt:If/>`.

## Контекст в тегах шаблонов

Контекст (место расположения тега) имеет важное значение в шаблонизаторе Movable Type. Некоторые теги контекстно-независимы (например, `<mt:BlogName/>`), но большинство тегов могут располагаться только в определённых местах.

Контекст — это расположение тега относительно других тегов и определённых шаблонов. Иногда разный контекст для одного тега может приводить к разным результатам. Пример:

```xml
<mt:Entries>
    <mt:EntryTitle/> <!-- Всё правильно, тег используется в соответствии с контекстом. -->
</mt:Entries>
<mt:EntryTitle/> <!-- Ошибка! Тег используется вне контекста -->
```

Первый тег `<mt:EntryTitle />` находится в контексте тега `<mt:Entries>`. Как упоминалось выше, `<mt:Entries>` выводит список записей, а в данном случае — список заголовков записей. Второй тег `<mt:EntryTitle/>` приведёт к ошиибке, так как он находится вне контекста тега `<mt:Entries>`. Шаблонизатор просто не поймёт, какой заголовок необходимо вывести. 

### Явный и неявный контекст

Рассмотренный выше пример — это явный контекст, когда тег был использован за пределами другого тега. Также существует неявный контекст, когда определённые теги могут (или не могут) быть использованы в определённых шаблонах. 

Например, блоковый тег `<mt:Entries>` на главной странице выведет один список записей, на странице с архивом по месяцам — другой, и на странице категории список записей будет отличаться от главной страницы, хотя во всех вариантах используется один и тот же тег. 

Такой механизм работы позволяет сократить количество кода в шаблонах, так как можно размещать один и тот же код в разных шаблонах, но работать он будет по-разному. Кроме того, в некоторых случаях не понадобятся блоковые теги для вывода содержимого. Например, в шаблоне записи не нужно добавлять блоковый тег `<mt:Entries>`, так как контекст здесь — индивидуальная запись. И, чтобы вывести заголовок этой записи, достаточно будет указать `<mt:EntryTitle/>`.
