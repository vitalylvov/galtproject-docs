# Контракт PlotManager - Документация

## Описание проблемы
Необходимо дать возможность любому Человеку, который владеет Землей - добавить свой `Земельный участок` в проект. При этом форма участка может быть произвольной.

## Цель Контракта
- Дать возможность любому человеку вступить в `Проект`.
- Дать возможность любому человеку записать участок для его последующей продажи через Краудсейл.
- Дать возможность учитывать участки произвольной формы.
- Дать возможность записать участок с целью продаджи или юридической поддержки в имущественных спорах.

## Общие условия
Действия будет выполнять веб-приложение. Вдалельцем Контракта PlotManager будет GALT Project Core Team. В Контракте будет храниться ХЕШ Фамилии, Имени, ID / Паспорта Владельца, а так же Кадастровый номер участка в той или иной правовой Юрисдикции. 
Контракт PlotManager будет иметь права на операции над контрактом SPACE - токен, пока система будет тестироваться. Потом эти права будут убраны! В случае ошибок GALT Project Core Team или третьи лица по ее поручению будут исправлять возникшие ошибки.

## Сценарий Контракта

## Статусы заявок

|Статус|Описание|
|------|--------|
|Новая|Устанавливается при создании Заявки|
|Готова к рассмотрению|Устанавливается при подписании транзакции на отправку заявки|
|На рассмотрении|Устанавливается при подписании транзакции Валидатором на получение Заявки|
|Утверждена|Устанавливается при подписании транзакции Валидатором на утверждение Заявки|
|Отклонена|Устанавливается при подписании транзакции Валидатором на отклонение Заявки|
|Возвращена|Устанавливается при подписании транзакции Валидатором на возвращение заявки на доработку Заявки|
|Разобрана заявителем|Устанавливается, если заявитель разобрал упаковку из заявки. Переходит из статуса `Новый`|
|Возврат комиссии|Устанавливается, если заявитель получил обратно комиссию. Переходит из статуса `Разобрана заявителем`|
|Получено вознаграждение валидатора||
|Закрыта||

### Сценарий 1: Создание и утверждение заявки на создание земельного участка - Реализация.

1. Пользователь через веб-интерфейс, подключенный к MetaMask вводит следующие параметры:

|Параметр|Значение|
|--------|--------|
|3-N точек границ Земельного участка в формате Геохеш - координат|[w24qgy48e](http://explorer.galtproject.io/map/#w24qgy48e), [w24qgsqw3](http://explorer.galtproject.io/map/#w24qgsqw3), [w24qgumbk](http://explorer.galtproject.io/map/#w24qgumbk) или GPS координат в формате Google Maps, например, 1.212032, 104.577743|
|Фамилия и Имя| Свою Фамилию и Имя|
|Номер ID / Паспорта|5478467392|
|Кадастровый номер участка в своей юрисдикции|630107140031259|
|Точность разбиения|11|
|Токен для оплаты комиссии|ETH или GALT|

Приложение не хранит Фамилия и Имя и Номер ID / Паспорта!

Приложение определяет разбиение этого участа на Геохеши с заданной точностью апроксимации в браузере и строит картинку.

Например:

|Геохеш|
|------|
|[w24qguf](http://explorer.galtproject.io/map/#w24qguf)|
|[w24qguc](http://explorer.galtproject.io/map/#w24qguc)|
|[w24qgv4](http://explorer.galtproject.io/map/#w24qgv4)|
|Другие 36 Геохешей|

Всего 39 геохешей.

2. Пользователь подписывает транзакцию на создание заявки. 

Вызывается метод. Создается новый SPACE - токен`Упаковки`. Новый токен создается всегда!

|Токен|Владелец|
|-----|--------|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|Контракт SplitMerge |

В контракте SplitMerge добавляется запись в маппинг CustomPlotBorders, который содержит соответствие id уникального участка - координата вершины. Подробнее - Контракт [SplitMerge](https://github.com/andromedaspace/galtproject-docs/blob/master/ru/contracts/SplitMerge.md#%D0%A1%D1%86%D0%B5%D0%BD%D0%B0%D1%80%D0%B8%D0%B9-3-%D0%A1%D0%BE%D0%B7%D0%B4%D0%B0%D0%BD%D0%B8%D0%B5-%D1%83%D0%BD%D0%B8%D0%BA%D0%B0%D0%BB%D1%8C%D0%BD%D1%8B%D1%85-%D1%83%D1%87%D0%B0%D1%81%D1%82%D0%BA%D0%BE%D0%B2).

|Токен SPACE|номер позиции|координата вершины|
|-----|--------| --------|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|1|[w24qgy48e](http://explorer.galtproject.io/map/#w24qgy48e) |
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|2|[w24qgsqw3](http://explorer.galtproject.io/map/#w24qgsqw3) |
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|3|[w24qgumbk](http://explorer.galtproject.io/map/#w24qgumbk) |

Создаются токен SPACE самого большого геохеша, который входит в Земельный участок. Владец токенов - PlotManager.
Если токен Геохеша уже создан и принадлежит контракты PlotManager, то он просто используется.

|Токен SPACE|Владелец|
|-----|--------|
|[w24qguf](http://explorer.galtproject.io/map/#w24qguf)|Контракт PlotManager|

Происходить запись в маппинг в контракте SplitMerge <id токена SPACE `Упаковки`> - <id токена SPACE `Геохеш - участка`>. Это фактически правила сборки или соответствие уникального участка - геохешам.

|id токена SPACE `Упаковки`|номер позиции|id токена SPACE `Геохеш-участка`|
|------------------|------------|-------|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|1|[w24qguf](http://explorer.galtproject.io/map/#w24qguf)|

Происходит запись в мапинг в контракте SplitMerge  <id токена SPACE `Геохеш - участка`> - <id токена SPACE `Упаковки`>

|id токена SPACE `Геохеш-участка`|номер позиции| id токена SPACE `Упаковки`|
|------------------|------------|-------|
|[w24qguf](http://explorer.galtproject.io/map/#w24qguf) |0|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|

Методами SplitMerge - токен [w24qguf](http://explorer.galtproject.io/map/#w24qguf) упаковывается в 743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk через обмен. 

|Токен SPACE|Владелец|
|-----|--------|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|Контракт PlotManager |
|[w24qguf](http://explorer.galtproject.io/map/#w24qguf)|Контракт SplitMerge |

И в итоге Владелец токена 743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk - контракт PlotManager. Создан и передан контракту SplitMerge токен [w24qguf](http://explorer.galtproject.io/map/#w24qguf).

Создается Заявка:

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Ethereum адрес заявителя| 0xFB7442a......69e3|
|Точки Границ участка массивом|[w24qgy48e](http://explorer.galtproject.io/map/#w24qgy48e), [w24qgsqw3](http://explorer.galtproject.io/map/#w24qgsqw3), [w24qgumbk](http://explorer.galtproject.io/map/#w24qgumbk)|
|Хеш ИМЕНИ ФАМИЛИИ Номер ID / Паспорта|70931C7045A38EA193822F8E4AFF8188128DAA2624AE9FA30BFE025DA5059C03|
|Кадастровый номер участка в своей юрисдикции|630107140031259|
|id `Упаковки`|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|
|Статус заявки|Новая|
|Страна|Код страны|

Заявка записывается в Контракт. Пользователь оплачивает возвратную комиссию за рассмотрение заявки. Сейчас это комиссия 1 ETH.

Если была выбрана комиссия в GALT, то оплачиваются GALT.

* Комиссия за услугу записи участка в блокчейн
    * Оплачивается при создании заявки
    * Равна ставке комиссии на момент создания заявки
    * Значение  задается при создании контракта
    * Может быть переназначена владельцем контракта PlotManager
    * Является возвратной, если заявка не переходила в статус `Готова к рассмотрению`
* Процент комиссии с одной заявки, получаемый владельцем контракта
    * Задается владельцем контракта
    * Может быть в любое время изменен
    * Рассчитывается на момент вывода валидатором средств
    * Задан в процентах от общей стоимости
    * Остаток комиссии может затребовать владелец контракта

3. Пользователь нажимает кнопку `Добавить геохеши в заявку`. Автоматически создается серия транзакций. В каждой транзакции указывается - ID заявки и массив геохешей.

Пользователь подписывает транзакции. Их может быть много.

Метод контракта PlotManager создает для всех переданных геохешей токен SPACE `Геохеш - участка.`

|Токен SPACE|Владелец|
|-----|--------|
|[w24qguc](http://explorer.galtproject.io/map/#w24qguc)|Контракт PlotManager|
|[w24qgv4](http://explorer.galtproject.io/map/#w24qgv4)|Контракт PlotManager|
| Еще 36 токенов|Контракт PlotManager|

Метод по ID заявки берет ID SPACE `Упаковки` 743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk и методом контракта SplitMerge производит запись в маппинги SplitMerge и передает токены контракту SplitMerge.

|id токена SPACE `Упаковки`|номер позиции|id токена SPACE `Геохеш-участка`|
|------------------|------------|-------|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|2|[w24qguc](http://explorer.galtproject.io/map/#w24qguc)|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|3|[w24qgv4](http://explorer.galtproject.io/map/#w24qgv4)|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|4-39|39 токенов SPACE |

|id токена SPACE `Геохеш-участка`|номер позиции| id токена SPACE `Упаковки`|
|------------------|------------|-------|
|[w24qguc](http://explorer.galtproject.io/map/#w24qguf) |0|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|
|[w24qgv4](http://explorer.galtproject.io/map/#w24qguf) |0|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|
|39 токенов SPACE |0|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|


|Токен SPACE|Владелец|
|-----|--------|
|[w24qguc](http://explorer.galtproject.io/map/#w24qguc)|Контракт SplitMerge|
|[w24qgv4](http://explorer.galtproject.io/map/#w24qgv4)|Контракт SplitMerge|
| Еще 36 токенов|Контракт SplitMerge|

В итоге все 39 токенов SPACE принадлежат контракту SplitMerge, а токен 743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk контракту PlotManager.

Если токены Геохешей уже есть и принадлежать контракту PlotManaget, то они не создаются, а просто добавляются в упаковку.

Если токены уже существуют и не принадлежат контракты PlotManager - то выдается ошибка.

4. Пользователь нажимает кнопку "Отправить заявку", отправляется транзакция.

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Статус заявки|Готова к рассмотрению|

После этого добавлять геохеши в Заявку и Упаковку нельзя!

5. В контракте есть маппинг адресов, которые имеют право утверждать Заявки. Маппинг записывается Владельцем контракта - GALT PROJECT CORE TEAM. Фактически это список адресов, которые могут создавать транзакцию с кадастровым номером, Фамилией и Именем для проверки. Назовем эти Адреса - `Валидатор Заявки`. Владелец контракта может удалять и добавлять валидаторов.

6. Валидатор видит в интерфейсе все заявки, которые имеют статус `Готова к рассмотрению`, если код страны в заявке = коду страны у Валидатора. Валидатор нажимает кнопку "Получить заявку", выполняется метод контракта PlotManager и валидатор назначается заявке. Статус заявки меняется.

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Статус заявки|На рассмотрении|
|Валидатор|0x346hsh.....678|

7. `Валидатор Заявки` через веб интерфейс и подключенный metamask видит свои текущие Заявки со статусом `На рассмотрении`. Он нажимает кнопку `Проверить данные`. Появляется форма для ввода данных. По кадастровому номеру находит в Кадастре той или иной страны Фамилию и Имя заявителя, его ID. 
Вводит: 
- точки Границ участка по Кадастру. 
- Фамилию и Имя, Номер ID / Паспорта. 

Проверяет, что границы Участка совпадают и Геохеши не выходят за границы земельного участка. Приложение выдает сообщение, что Хеш Фамилии, Имени и ID совпадают и что границы участка совпадают. Появляется кнопка `Утвердить заявку`, `Вернуть заявку` или `Отклонить заявку`.

8. Валидатор нажимает `Утвердить заявку` и  Подписывает транзакцию. Статус заявки меняется на Утверждена.

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Статус заявки|Утверждена|

9. Токен 743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk передается пользователю.

|Токен SPACE|Владелец|
|-----|--------|
|743883bs7GEOw24qgy48hgdhhqgddqw663w24qgumbk|Заявитель 0xFB7442a......69e3 |

10.  В текущей версии на одну заявку создается строка одна упаковка, если пользователь хочет добавить еще участок, то это должна быть отдельная заявка.


Далее - контракт [CreateTerritory](CreateTerritory.md)

### Сценарий №2 - Добавление Геохешей в заявку и удаление Геохешей из заявки - Реализация.

1. Пользователь нажимает кнопку "Мои заявки". Пользователь видит все заявки с возможностью фильтрации по статусам. При нажатии на заявку отображается участок упаковки на карте и геохеши, которые в него входят.

2. Если Заявка имеет статус `Новая` или `Возвращена` , то отображается кнопка `Добавить геохеш`. При нажатии отображается форма для ввода 1-N геохешей и кнопка `Добавить`. При вводе геохешей - они отображаются и подсвечиваются на карте. При вводе каждого геохеша определяется есть ли у него сосед, который уже включен в упаковку. Если соседа нет - то отображается сообщение о невозможности включить геохеш в упаковку. Некорреткный геохеш подсвечивается на карте. Упаковка из заявки и все ее дочерние геохеши при этом тоже отображаются.

3. При нажатии кнопки `Добавить` - геохеши, которые могут быть добавлены - записываются в массив и создается серия транзакций с вызовом методо контракта PlotManager по добавлению в упаковку по номеру заявки новых геохешей, который в свою очередь вызывает метод контракта SplitMerge.  Происходят все необходимые проверки. Геохеши должны принадлежать контракту PlotManager.

4. Геохеши от контракта PlotManager переходят контракту SplitMerge, происходят записи в мапинги и в счетчик количества геохешей в упаковке.

5. Если Заявка имеет статус `Новая` или `Возвращена`, то отображается кнопка `Удалить геохеш`. При нажатии отображается список геохешей упаковки. Пользователь выбирает геохеши, они отображаются на карте. Выбрав несколько геохешей Пользователь нажимает кнопку `Удалить`. Генерится серия транзакций на удаление. Счетчик геохешей упаковки уменьшается, токены геохешей передаются от контракта SplitMerge контракту PlotManager.


### Сценарий №3: Изменение данных по заявке - Реализация

1. Для заявок со статусом `Новая` или `Возвращена` отображаются кнопки на изменение полей заявки. 

2. При нажатии появляется возможность отредактировать поле, подтвердить редактирование, создать и подписать транзакцию.

### Сценарий №4: Проверка Геохешей упаковки по заявке и исправление - Реализация

1. Для заявок со статусом `Новая` отображается кнопка `Проверить геохеши упаковки`.

2. Пользователь нажимает кнопку. Происходит построение сетки геохешей по участку и проверка все ли они помешены в упаковку.

3. Если есть геохеши, которые не помещены, то появляется кнопка `Добавить геохеши`.

4. Создается серия транзакций, после подписания которых геохеши добавляются в упаковку по заявке.

### Сценарий №5: Разбор упаковки Заявителем  - Реализация

1. Пользователь нажимает кнопку "Мои заявки". Пользователь видит все заявки с возможностью фильтрации по статусам. При нажатии на заявку отображается участок упаковки на карте и геохеши, которые в него входят.

2. Если Заявка имеет статус `Новая`, то отображается кнопка `Разобрать Геохеш`.

3. Если пользователь нажимает кнопку, то создается серия транзакций по удалению геохешей из упаковки, пока счетчик геохешей для упаковки не будет равен 0. Удаление - процесс передачи токена геохеша от контракта SplitMerge контракту PlotManager.

4. После того, как счетчик геохешей становится равным 0, статус заявки меняется не `Разобрана Заявителем`.

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Статус заявки|Разобрана Заявителем|

### Сценарий №6: Возвращение комиссии заявителем, в случае разбора упаковки в заявке в статусе `Новая` - Реализация

1. Для заявок со статусом `Разобрана заявителем` отображается кнопка `Вернуть комиссию`. 

2. Ползователь нажимает кнопку, подписывает транзакцию и получает обратно комиссию. Заявка переходит в статус `Возврат комиссии`.

3. Транзакция на возврат комиссии выполняется только в том случае, если Заявка имеет статус `Разобрана заявителем`.

4. Коммиссия возвращается в тех токенах, которые были указаны в заявке: GALT или ETH.

### Сценарий №7: Возврат заявки на доработку - Реализация

1. Валидатор нажимает кнопку `Вернуть заявку`. Появляется поле для ввода текста. Валидатор вводит информации о причинах возврата и Нажимает кнопку `Подтвердить возврат`, подписывает транзакцию. Статус заявки меняется.

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Статус заявки|Возвращена|


### Сценарий №8: Отклонение заявки - Реализация

1. Валидатор нажимает кнопку `Отклонить заявку`. Появляется поле для ввода текста. Валидатор вводит информации о причинах отклонения и Нажимает кнопку `Подтвердить отклонение`, подписывает транзакцию. Статус заявки меняется. Текст причины отказа фиксируется в контракте.

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Статус заявки|Отклонена|


### Сценарий №9: Разбор упаковки Валидатором - Реализация

1. Валидатор видит в интерфейсе все заявки, которые имеют статус `Отклонена` / `Возвращена`  и которые были назначены ему.

2. Для этих заявок отображается кнопка `Разобрать Геохеш`.

3. Если Валидатор нажимает кнопку, то создается серия транзакций по удалению геохешей из упаковки, пока счетчик геохешей для упаковки не будет равен 0. Удаление - процесс передачи токена геохеша от контракта SplitMerge контракту PlotManager.

4. После того, как счетчик геохешей становится равным 0, статус заявки не меняется!

### Сценарий №10: Получение вознаграждения Валидатором - Реализация

1. Для валидатора для заявок со статусом `Утверждена` и `Отклонена` (если количество геохешей в упаковке равно 0) отображается кнопка `Получить награду`.

2. Валидатор нажимает кнопку. Подлписывается и отправляется транзакция. Происходит проверка количества геохещей в упаковке по заявке. Если не равно 0, то транзакция отклоняется. Если транзакция выполняется - Валидатор получает вознаграждение. Статус заявки меняется на `Получено вознаграждение валидатора`.

3. Вознаграждение оплачивается в тех токенах, которые были указаны в заявке: GALT или ETH.

### Сценарий №11: Получение вознаграждения Galt Space - Реализация

1. Для заявок со статусом `Получено вознаграждение валидатора` отображдается кнопка `Получить вознаграждение Galt Space` для адреса Galt Space.

2. Подписывается и отправляется транзакция, происходит проверка по статусы и выплата вознаграждения. Заявка переходит в статус `Закрыта`.

|Параметр|Значение|
|--------|--------|
|id Заявки|DBn4y2a......35b1|
|Статус заявки|Закрыта|

3. Вознаграждение оплачивается в тех токенах, которые были указаны в заявке: GALT или ETH.


### Сценарий №12: Установка допустимых токенов для оплаты заявки, стоимости и размеров вознаграждений - Не реализовано.

1. Galt Space может устанавливать, какие токены (ETH или GALT) могут быть приняты, как оплата за валидацию заявки.

2. В контракте есть разные методы по оплате комиссии в GALT и ETH, во возврату комиссии и получению вознаграждения.

3. Galt Space через интерфейс вызывает метод контракта PlotManager и может для каждого токена устанавливать статус `Активен`/`Не активен`. Если способ оплаты не активен, то транзакция на создание заявки отклоняется.

4. Для этого есть раздел `Администрирование`. При нажатии на него отображаются кнопки, одна из них - `Токены для оплаты заявки.`

5. Отображается список токенов: GALT, ETH. 

|Токен|Статус приема|Размер комиссии|Вознаграждение валидатора|Вознаграждение Galt Space|
|----|--------|-----|-----|------------|
|ETH|Принимается|1|50%|50%|
|GALT|Принимается|100|60%|40%|

У каждого токена есть переключатель `Активен`/`Не активен`, размер комиссии и проценты, на которые делится комиссия между Валидатором и Galt Space. Если переключатель меняет положение и/или меняется
значение комиссии или процентов, то становится активна кнопка `Записать`. Создается, подписывается и выполняется транзакция. Данные обновляются после подтверждения транзакции.

### Сценарий №13: Добавление или удаление валидатора - Не реализовано.

1. Galt Space может устанавливать список адресов Валидаторов, которые могут подтверждать заявки, их тип и дополнительные данные.

2. Galt Space может устанавливать количество валидаторов, которые нужны для принятия заявки и их тип.

3. В интерфейсе Galt Space или владельцы Galt Space видят кнопку `Администрирование`. При нажатии отображается кнопка `Управление списком валидаторов`. 

4. При нажатии отображаются 2 таблицы.

5. Первая таблица - Типы валидаторов, необходимые для утверждения заявки.

|Тип валидатора|Статус|
|---------|----------|
|Юрист|Активен|
|Геодезист|Не Активен|

Отображается количество валидаторов, необходимых для утверждения заявки.

При изменении статуса - создается транзакция.

Так же должна быть кнопка `Добавить`. При нажатии появляется поле для ввода Текста типа валидатора. После заполнения поля нажимается кнопка `Подтвердить добавление`, создается и подписывается транзакция. 
Количество активных статусов определяет, сколько нужно Валидаторов, чтобы утвердить или Отклонить заявку. Если активных статусов 2, то нужно два утверждения, если 1,то 1.

6. Вторая таблица.

|Адрес Валидатора|Название|Тип|Код страны|Расположение| Статус |
|----------|---------|----------|--------|-----------|-----------|
|0xJjdj4884.....783|Компания Шамшанов и Партнеры|Юрист|Ru|zexub373n|Активен|
|0xJjdj4884.....723|Геодезист Иван|Геодезист|Ru|zexub372n|Не Активен|

Для каждой записи есть кнопка `Изменить статус`. При нажатии создается Транзакция и меняется статус Валидатора.

Есть кнока `Добавить Валидатора`. При нажатии появляются поля:

|Поле|Пример Значения|Тип|
|------|-------|------|
|Адрес Валидатора|0xJjdj4884.....71b|Адрес|
|Название|Компания Шамшанов и Партнеры|Строка|
|Тип|Юрист|Значение из контракта|
|Код страны|RU|Значение из контракта|
|Расположение|zexub373n|Координаты в формате Геохеш или GPS координат|

Все поля обязательные.
После заполнения полей создается и подписывается транзакция на вызов метода контракта PlotManager. Новый Валидатор добавляется в контракт.


### Сценарий №14: Подписание транзакций при помощи встроенного кошелька - Не Реализовано

1. В верхнем отображается кнопка `Мой кошелек` и рядом баланс в эфирах.

2. При нажатии на кнопку отображается в всплывающем окне Адрес(ссылка на Etherscan), список транзакций со статусом и ссылки на них на Etherscan и кнопка `Скопировать приватный ключ`/`Импортировать приватный ключ`. При нажатии ключ копируется в буфер.

3. Во всех деййствиях, описанных выше, если количество транзакций больше 5, то отображается всплывающее окно.

4. В окне отображается надпись "Вам нужно подписать X транзакций. Это много. Для подписания транзакций нужно X ETH на газ. Вы можете использовать встроенный кошелек приложения. Нажимая кнопку Подтвердить, Вы отправите ETH на адрес <>. Остаток будет возвращен Вам после выполнения всех транзакций. Скопируйте на всякий случай приватный ключ." Три кнопки: `Подтвердить`, `Отказаться и подписать самому`, `Скопировать приватный ключ`.

5. Если нажато Подтвердить, то генерится транзакция на перечисление Эфиров с кошелька, подключенного к метамаску. Транзакция подписывается и генерятся все необходимые транзакции, которые будет выполнять сервис после поступления Эфиров.

6. Если нажато Отказаться и подписать самому, то генерятся все транзакции и пользователь подписывает их сам.

7. Если нажато Скопировать приватный ключ, то копируется ключ в буфер.

8. Все должно работать на клиенте.

# Беклог

1. Сделать так, чтобы Заявитель сам выбирал валидатора или валидаторов по расстоянию от одного из геохешей участка до Валидатора. Либо, чтобы Валидаторы видели заявки, которые можно взять исходя из этого.

2. Сделать возможность наличия разных типов валидаторов, определения количества валидаторов с определенным типом на заявку.
