---
title: Документация API SelSup

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell

toc_footers:
  - <a href='https://selsup.ru/application/integration/pageApi'>Добавить ключ API</a>
  - <a href='https://api.selsup.ru/all.html'>Список всех методов</a>
  - <a href='https://github.com/slatedocs/slate'>Сделано в Slate</a>

search: true

code_clipboard: true

meta:
  - name: description
    content: API SelSup — программный интерфейс для работы с сервисом SelSup. Даёт возможность работать со всеми функциями SelSup из вашей системы. Для использования API добавьте новый токен на странице:https://selsup.ru/application/integration/pageApi. Добавленный токен необходимо передавать во всех запросах к API SelSup, в заголовке Authorization. API можно использовать без ограничений на тарифе Выделенный сервер.
---

# Введение

SelSup API — программный интерфейс для работы с сервисом SelSup. Он даёт возможность обмениваться информацией между системой продавца и SelSup.

Методы API позволяют использовать весь функционал сервиса SelSup для работы с маркетплейсами Ozon, Wildberries, Aliexpress, Яндекс.Маркет, СберМегаМаркет и Авито.

SelSup позволяет создавать карточки на всех маркетплейсах, заполнять параметры, вести учет остатков товаров, принимать заказы по FBS с маркетплейсов и интернет-магазина, обновлять остатки на позиции, по которым пришел заказ. Вести аналитику продаж и учет финансов.

С помощью API вы можете подключить любые источники заказов к SelSup и вести быстрый учет остатков с маркетплейсов, сайта и других источников заказов.

Данная документация в разработке, описаны пока не все методы. Вы можете посмотреть полный список методов по ссылке:
<a href='https://api.selsup.ru/all.html'>https://api.selsup.ru/all.html</a>

По умолчанию GET запросы используются для получения данных, все запросы на изменение данных отправляются методом POST

# Авторизация

> Как передавать токен авторизации в запросах

```shell
# With shell, you can just pass the correct header with each request
curl "https://selsup.ru/<host>/api/product/findProduct" \
  -H "Authorization: <token>"
```

> Проверьте, что у вас указан ваш базовый адрес сервера вместо &lt;host&gt; и &lt;token&gt; заменен на ваш токен API.

Перейдите на страницу настройки API:
<a href='https://selsup.ru/application/integration/pageApi'>https://selsup.ru/application/integration/pageApi</a>

Введите название нового токена в поле и нажмите кнопку Добавить токен. Название должно быть уникальным в рамках вашего аккаунта. Рекомендуется для разных сервисов использовать свои токены, чтобы в любой момент можно было отозвать токен.

Токен необходимо передавать в заголовке Authorization: <токен>

Адрес для запросов API так же указан на странице выше в тексте: "Используйте базовый адрес, вашего сервера: ..."

Например: https://selsup.ru/<ваш сервер>/api/

# Заказы

## Создание заказа с сайта

<a href="https://api.selsup.ru/all.html#tag/Zakazy/operation/createOrder">Полный список полей</a>

```shell
curl -X POST "https://selsup.ru/<host>/api/order/createOrder" \
  -H "Authorization: <token>" -data '{
  "name": "Название заказа если нужно уникальность не проверяется",
  "type": "RETAIL",
  "stockWarehouseId": 0,
  "deliveryDate": "2019-08-24T14:15:22Z",
  "externalOrderId": "Номер заказа в интернет-магазине или FBS заказа",
  "trackingNumber": "Необязательный трек номер отправления",
  "organizationId": 0,
  "customerId": 0,
  "customer": {
  }
  "products": [
    {
      "productId": 0,
      "quantity": 0,
      "price": 0
    }
  ],
  "comment": "Необязательный комментарий",
  "deliveryService": "CDEK"
}'
```

> В результате отдается JSON заказа, с проставленным значение id у заказа и у позиций заказа

```json
{
  "id": 1,
  "createdDate": "Дата создания заказа",
  "products": [
    {
      "id": 1
    }
  ]
}
```

Вы можете передавать новые заказы в SelSup по API, например с вашего интернет-магазина. В запросе необходимо передавать информацию о заказе и список товаров в заказе. В качестве уникального ключа, для того, чтобы не создавать дубликаты заказов используйте externalOrderId - номер заказа на сайте интернет-магазина.

Поле organizationId нужно обязательно передавать, если у вас в аккаунте несколько организаций.

Для передачи товаров необходимо предварительно связать товары сайта, с товарами в SelSup, чтобы потом передавать productId - идентификатор товара в SelSup. Сделать это можно разовов, импортировав все товары методом findProduct

В позиции заказа в товаре quantity обязательно нужно передавать, как и цену товара price.

### Тело запроса JSON с ключами

Параметр  | Тип | Обязательный | Описание
--------- | ------- | ------- | -----------
type | enum | Да | Тип заказа, для заказов с сайта необходимо ставить "RETAIL"
products | array of OrderProduct | Да | Список товаров заказа
writeOffStocks | boolean | Нет | Список остатки товаров под заказ со склада SelSup
stockWarehouseId | int64 | Нет | Идентификатор вашего склада в SelSup, с которого списать остатки товаров. Отдается в методе getApplicationData.
organizationId | int64 | Нет | Идентификатор организации. Необязательный если в аккаунте 1 организация. Отдается в методе getApplicationData.
comment | string | Нет | Текстовый комментарий к заказу
name | string | Нет | Название заказа для быстрого поиска в списке

## Создание отгрузки на маркетплейс

```shell
curl -X POST "https://selsup.ru/<host>/api/order/createOrder" \
  -H "Authorization: <token>" -data '{
  "name": "Название заказа если нужно уникальность не проверяется",
  "type": "FBM",
  "warehouseId": 0,
  "externalOrderId": "Номер заказа на маркетплейсе",
  "organizationId": 0,
  "stockWarehouseId": 0,
  "writeOffStocks": true,
  "products": [
    {
      "productId": 0,
      "quantity": 0
    }
  ],
  "comment": "Необязательный комментарий"
}'
```

> В результате отдается JSON заказа, с проставленным значение id

```json
{
  "id": 1,
  "createdDate": "Дата создания заказа",
  "products": [
    {
      "id": 1
    }
  ]
}
```

Вы можете передавать новые заказы в SelSup по API, например с вашего интернет-магазина. В запросе необходимо передавать информацию о заказе и список товаров в заказе. В качестве уникального ключа, для того, чтобы не создавать дубликаты используется externalOrderId


### Запрос

`POST https://selsup.ru/<host>/api/order/createOrder`

### Тело запроса JSON с ключами

Параметр  | Тип | Обязательный | Описание
--------- | ------- | ------- | -----------
type | enum | Да | Тип заказа, для заказов с сайта необходимо ставить "RETAIL"
products | array of OrderProduct | Да | Список товаров заказа
stockWarehouseId | int64 | Нет | Идентификатор вашего склада в SelSup, с которого списать остатки товаров. Отдается в методе getApplicationData.
organizationId | int64 | Нет | Идентификатор организации. Необязательный если в аккаунте 1 организация. Отдается в методе getApplicationData.
comment | string | Нет | Текстовый комментарий к заказу
name | string | Нет | Название заказа для быстрого поиска в списке

### Возможные ошибки

> В результате отдается JSON заказа, с проставленным значение id

```json
{
  "message": "error_empty_warehouse"
}
```

В случае ошибки отдается код ответа 400, а в теле отдается message - с кодом ошибки и messageParams - дополнительные параметры сообщения об ошибке

Сообщение | Причина
--------- | -------
error_empty_warehouse | Не указан параметр warehouseId для заказа на маркетплейс type=FBM
error_no_organization | Не указано поле organizationId, если в аккаунте несколько организаций
error_no_marketplace | Не указан service, для type=FBS или type=FBM заказов
error_no_quantity_for_order_product | Не указано количество товара у позиции заказа products[index].quantity
error_no_price_for_order_product | Не указана цена у позиции заказа products[index].price

## Получение списка заказов

```shell
curl "https://selsup.ru/<host>/api/order/findOrder?type=FBS&actual=true" \
  -H "Authorization: <token>"
```

Позволяет получить список заказов с фильтрацией и поиском. В данном методе содержимое заказа не отдается сразу, чтобы получить заказы сразу с товарами, используйте другой метод /api/fbs/findOrder.

### Параметры запроса

Параметр | Тип | Описание
--------- | ------- | -----------
query | string | Поисковый запрос по номеру заказа
actual | boolean | Отображать только актуальные заказы - не закрытые
type | enum | Тип заказа строкой: "FBM" - отгрузка на маркетплейс, "FBS" - заказ с маркетплейса со склада продавца, "FBO" - заказ с маркетплейса со склада самого маркетплейса, "RETAIL" - розничный заказ, в том числе с сайта, "WHOLESALE" - оптовый заказ
organizations | Array of integers <int64> uniq | Оргиназции по которым отдать заказы
service | enum | Сервис или маркетплейс заказа: "NONE", "WILDBERRIES", "OZON", "YANDEX_MARKET", "FAMILIYA", "NATIONAL_CATALOG", "ALIEXPRESS", "OTHER", "MOY_SKLAD", "SBER_MEGA_MARKET", "CISLINK" "ONE_C", "AVITO", "LEROY_MERLIN", "DETMIR", "KAZAN_EXPRESS"
productId | int64 | Идентификатор товара в SelSup, который есть в заказе
