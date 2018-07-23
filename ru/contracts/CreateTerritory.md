### В разработке
# Ввод новой территории - Документация

## Описание проблемы
Согласно общей схеме аукциона каждый может создать заявку на аукцион и выиграть участок земли за токены GALT.
Для того, чтобы управлять тем, какие участки можно ставить на аукцион, а какие нет вводится понятие Территории. Территория - это токен стандарта ERC721, который принадлежит контракту аукциона на землю. Эти токены выполняют функцию white - листа и используются для проверки того, что геохеш, который Пользователь хочет поставить на аукцион - допустим.
Приведем пример есть геохеш [w24qg]( http://explorer.galtproject.io/map/#w24qg ). Это Территория.
Территория обладает несколькими свойствами:
- она принадлежит контракту Аукциона;
- ее нельзя передать другому владельцу;
- ее нельзя разделить или объединить;
- она не имеет залога в токенах GALT;

Предположим пользователь хочет создать аукцион и приобрести участок с геохешем [w24qge](http://explorer.galtproject.io/map/#w24qge). Геохеш [w24qge](http://explorer.galtproject.io/map/#w24qge)  входит в [w24qg]( http://explorer.galtproject.io/map/#w24qg ) и соответствено Пользователь может создать на него заявку и выиграть на аукционе.
Если пользователь попытается создать заявку на [w24qex](http://explorer.galtproject.io/map/#w24qex), то он не сможет это сделать так как данный участок находится за пределами допустимой Территории. В нашем случае аналогом Территории является допустимое пространство имен.

При этом возникает проблема, как и на каких основаниях добавлять новые Территории.
Введем несколько терминов:
- Владелец Территории - физическое лицо, юридическое лицо, экономический агент, который владеет участком земли в той или иной юрисдикции. Может при помощи тех или иных юридических механизмов продать землю участникам Galt Project. Имеет свой адрес в сети Ethereum и получает Эфиры за землю.
- Оператор ввода Территории в проект - физическое лицо, юридическое лицо, экономический агент, который предлагает включить Территорию в Galt Project. Имеет свой адрес в сети Ethereum и получает Эфиры от Владельца Территории, как комиссионное вознаграждение.

## Цели контракта
- создать механизм ввода новых территорий в проект;
- обеспечить возможность вознаграждения текущего владельца земли;
- защитить владельцев токенов GALT и владельцев земли (владельцев токенов SPACE) от мошенничества и размытия своих инвестиций;
- создать мотивацию для независимых Операторов ввода Территории искать и вводить новые территориии в проект;

## Общая схема
- Каждый (Оператор ввода территории) должен иметь возможность создать заявку на создание новой территории и включение ее в проект. 
- В случае, когда территория принадлежит владельцу она должна быть фактически продана участникам проекта. 
- Участники проекта должны сделать все необходимые проверки и юридические действия, принять решение и выкупить эту землю. 
- Выкуп должен осуществляться либо за счет участников проекта, либо за счет средств новых инвесторов. 
- В результате выкупа владелец получает средства за свою землю, а участники проекта - возможность получить новые участки на аукционе.

## Сценарий контракта

### Сценарий №1 Создание заявки на новую территорию на выкуп через дополнительную эмиссию GALT
1. Пользователь А (Оператор ввода Территории) может вызвать метод CreateNewTerritoryProposal() и создать заявку на продажу новой территории. В заявке пользователь указывает:
- геохеш территории или несколько геохешей;
- минимальный размер участка на аукционе в количестве символов геохеша;
- максимальный размер участка на аукционе в количестве символов геохеша;
- стоимость Территории в Эфирах;
- количество выпускаемых под Территорию GALT;
- Длительность Генезиса - нового выпуска GALT под территорию;
- адрес продавца (текущего владельца земли - Владельца Территории) для перечисления Эфира;
- процент получаемого вознаграждения за землю Владельцем Территории;
- адрес фонда для получения оставшейся части инвестиций;
- размер комиссионного вознаграждения для Оператора ввода Территории;

| Параметр | Значение | Комментарий |
|---------|-----------| ----------- |
| Геохеш территори| [w24qg]( http://explorer.galtproject.io/map/#w24qg ) и [w24qex](http://explorer.galtproject.io/map/#w24qex) | Новая большая территория для проекта. Задается одним или несколькими геохешами.|
| Максимальный размер участка на аукционе| 7 |	Примерный размер ≤ 153m	×	153m Например, этот участок [24qgvd](http://explorer.galtproject.io/map/#w24qgvd) |
| Минимальный размер участка на аукционе| 8 | Примерный размер ≤ 38.2m	×	19.1m Например, [24qgvdс](http://explorer.galtproject.io/map/#w24qgvdc) |
| Стоимость Территории в Эфирах| 15 000 | Эта сумма, которую хочет получить владелец за передачу прав на всю землю [w24qg]( http://explorer.galtproject.io/map/#w24qg ) и [w24qex](http://explorer.galtproject.io/map/#w24qex)|
| Количество выпускаемых под Территорию GALT| 15 000 000 | Эта сумма GALT, которая будет выпущена для привлечения средств инвесторов для выкупа Территории [w24qg]( http://explorer.galtproject.io/map/#w24qg ) и [w24qex](http://explorer.galtproject.io/map/#w24qex)|
| Длительность Генезиса | 90 дней |        |
| Процент получаемого вознаграждения за землю | 90 |Это означает, что из 15 000 Эфиров текущий владелец земли получит 90% или 13 500 Эфиров за вычетом вознаграждения Оператора ввода Территории. Если вознаграждение определено.|
| Адрес продавца (текущего владельца земли) | <адрес> |адрес, куда должны быть перечислены Эфиры. Это адрес текущего владельца, который хочет отказаться от земли в пользу проекта. |
| Адрес фонда | <адрес> | Если привлекаемые эфиры распределяются в определенном отношении между текущим владельцем земли и Фондом, то здесь указыватся адрес фонда. В этом примере Фонд получает 10% или 1500 Эфиров. Это Фонд, который берет на себя ответственность именно за эту Территорию. |
| Размер получаемого вознаграждения за землю Оператором ввода Территории | 1350 |Это означает, что из 13 500 Эфиров, которые идут на выкуп Территории,  Оператор ввода Территории получит 1 350 Эфиров или 10%.|

2. Создается заявка. Все владельцы земли голосуют своей репутацией по этой заявке. Каждый голос увеличивает значение переменной TerritoryPropVote. 
Например, пользователь А имеет репутацию 1340. По заявке он может проголосовать за или против.
Пусть пользователь проголосовал ЗА. В таком случае:

| Параметр | Значение до | Значение после |
|---------|-----------| ----------- |
|TerritoryPropVote |10 000| 11 340 |

Если получено 66% от общей репутации всех участников, то заявка принимается. Один адрес может проголосовать по одной заявке только один раз.

| Заявка  | Общая репутация | TerritoryPropVote | Процент |
|---------|---------|-----------| ----------- |
|id заявки|100 000 000 |68 000 000| 68% |

3. Если заявка была принята, то контракт создает новый контракт Генезиса на выпуск новых GALT. Контракт продлится 90 дней. Каждый может отправить Эфиры на контракт и получить в обмен GALT по фиксированному курсу. Если за 90 дней не будет собрано 15 000 Эфиров, то выпущенные токены GALT будут уничтожены, а Эфиры будут возвращены владельцам. Подробнее - см. Документацию контракта.
4. Если Генезис пройдет успешно. т.е. будет собрано 15 000 Эфиров, то Владелец Территории получит 12 150 Эфиров, Оператор ввода территории 1350 Эфиров, а Фонд, который будет отвечать за развитие этой Территории([w24qg]( http://explorer.galtproject.io/map/#w24qg ) и [w24qex](http://explorer.galtproject.io/map/#w24qex)) получит 1500 Эфиров. Будет запущен аукцион на земельные участки. Каждый при помощи GALT сможет сделать ставку на аукционе Викри на участок земли. По истечению срока аукциона победитель получить токен ERC721 участка земли и станет его владельцем. Выигравшая ставка в токенах GALT будет передана Фонду в качестве залога за участок и будет Репутацией владельца участка, которой тот будет голосовать. Подробнее - см. Документацию контракта.

## Неоднозначные вопросы
### Кто должен голосовать за ввод новых территорий?
Теоритически голосовать могут:
- владельцы земельных участков (держатели токенов SPACE стандарта ERC721);
- делегаты фондов;
- владельцы токенов GALT стандарта ERC20;

Нужно принять решение о том, кто из этих групп должен участвовать в голосовании, а кто нет.
### Какой процент голосов должен быть набран для ввода новых территорий?
### Кто должен владеть токеном Территории?
Токеном территории должен владеть контракт Генезиса на эту территорию или контракт аукциона на эту территорию.
### Сколько нужно контрактов Генезиса и контрактов аукциона?
На каждую новую территорию будет создаваться отдельный контракт генезиса на привлечение инвестиций и контракт аукциона на распределение это территории.
### Какие функции Оператора ввода Территории в проект?
### Как будет юридически оформлена передача земли от Владельца Территории проекту?