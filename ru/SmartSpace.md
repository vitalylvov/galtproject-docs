# Smart Space контракты

* Epic issue https://github.com/andromedaspace/WIJG/issues/358

## Цель
Предоставить возможность распределять репутацию одного Space токена между несколькими пользователями.

## Кейсы

* Совместная собственность
* Аренда/Лизинг

## Контракты, предоставляемые нашей командой
* [SpaceRentalBasic](SmartSpace-Rental.md) - базовый контракт аренды Space токена
* [SharedSpaceBasic](SmartSpace-SharedSpace.md) - базовый контракт совместной собственности Space токена

## Разработка кастомных решений

Пользователь может разработать свой контракт с любой логикой, которую он хочет реализовать. Контракт должен удовлетворять следующим требованиям:

* Собственником Space токена должен являться контракт
* Для распределения репутации Space токена вызывать соответсвтующие методы контракта Reputation