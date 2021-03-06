# Доступы контрактов

## Описание проблемы
В условиях работы контрактов в DAO необходимо определиться - у какого контракта - кто должен быть владельцем, и как будет осуществляться управление всей этой структурой.
Дополнительные сложности возникают изза наличия смежных DAO.

## Список Контрактов
### Глобальные контракты
В системе существует только один экземпляр глобальных контрактов

#### DAORegistry
* Создатель - Deployment Team
* Владелец - нет
* Обновляемый - нет

##### Предоставляет права
###### Публичные
* Создать и зарегистрировать в реестре новый фонд (MultiSig/Controller/Voting), заплатив соответствующую комиссию
* Создать и зарегистрировать в реестре новый контракт SmartSpace
* Проверить, может ли адрес минтить Space токены (как правило, проверка, является ли адрес активным контроллером)

###### Ограниченные
* Любой мультисиг может зарегистрировать в реестре spaceToken2daoMultisigMap созданный Space токен
* Кто имеет право на переназначение владельца токена?

#### SpaceToken
* Создатель - Deployment Team
* Владелец - нет
* Обновляемый - нет

#### GaltToken
* Создатель - Deployment Team
* Владелец - нет
* Обновляемый - нет

#### GALTGenesis
* Создатель - Deployment Team
* Владелец - нет
* Обновляемый - нет

#### SpaceGenesis
* Создатель - Deployment Team
* Владелец - нет
* Обновляемый - нет

#### GlobalReputation
* Создатель - Deployment Team
* Владелец - нет
* Обновляемый - нет

##### Предоставляет права
###### Публичные
* Кол-во всей выпущенной репутации
* Информацию о рептуации в разрезе `аккаунт` => `репутация (GALT)`
* Информацию о рептуации в разрезе `геохеш` => 
  * выпущен ли Space токен под него
  * залоге в GALT
  * репутации геохеша в разрезе `совладелец` => `залог совладельца в GALT`

###### Ограниченные
* Выпуск репутации (mint) разрешается только контракту аукциона
* Сжигание репутации (burn) разрешается только ???
* Переназначение всей репутации Space токена другому адресу разрешается контракту Space токена
* Переназначение репутации в рамках одного Space токена разрешается только владельцу этого токена


### Контракты фонда
На каждый фонд создается отдельная группа контрактов Controller/MultiSig/Voting

#### Controller
Контракт определяет права всех фондов

* Создатель - DAORegistry
* Владелец - MultiSig фонда
* Обновляемый - да

##### Предоставляет права
###### Публичные
* Является точкой входа для внешних контрактов

###### Ограниченные
* Любой делегат может создать предложение для голосования и зарегистрировать его


#### MultiSig
* Создатель - DAORegistry
* Владелец - MultiSig фонда
* Обновляемый - нет

##### Предоставляет права
###### Публичные
* Кол-во ETH, Galt токенов собственности
* Список Space токенов в собственности
* Получить список всех предложений делегатов

###### Ограниченные
* DAORegistry может перемещать GALT токены между мультисигами фондов без ограничений
* Установить новый список делегатов может только контракт голосования
* Контроллер может управлять всеми ETH и токенами
* Контроллер может зарегистрировать предложение делегата в списке

#### Voting
* Создатель - DAORegistry
* Владелец - MultiSig фонда
* Обновляемый - нет

##### Предоставляет права
###### Публичные
###### Ограниченные
* Контроллер может вызвать операцию пересчета голосов

### Вспомогательные контракты
#### Proposal
* Создатель - Controller фонда
* Владелец - MultiSig фонда
* Обновляемый - нет

##### Предоставляет права
###### Публичные
* Чтение предложения
* Чтение информации о статусе голосования

#### Ограниченные
* Любой делегат может проголосовать за любое предложение в статусе #идет голосование
* Любой адрес может исполнить одобренное решение, если пройден таймаут на исполнение