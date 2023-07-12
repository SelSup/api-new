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

SelSup API — программный интерфейс для работы с сервисом SelSup. Он даёт возможность обмениваться информацией между
системой продавца и SelSup.

Методы API позволяют использовать весь функционал сервиса SelSup для работы с маркетплейсами Ozon, Wildberries,
Aliexpress, Яндекс.Маркет, СберМегаМаркет и Авито.

SelSup позволяет создавать карточки на всех маркетплейсах, заполнять параметры, вести учет остатков товаров, принимать
заказы по FBS с маркетплейсов и интернет-магазина, обновлять остатки на позиции, по которым пришел заказ. Вести
аналитику продаж и учет финансов.

С помощью API вы можете подключить любые источники заказов к SelSup и вести быстрый учет остатков с маркетплейсов, сайта
и других источников заказов.

Данная документация в разработке, описаны пока не все методы. Вы можете посмотреть полный список методов по ссылке:
<a href='https://api.selsup.ru/all.html'>https://api.selsup.ru/all.html</a>

По умолчанию GET запросы используются для получения данных, все запросы на изменение данных отправляются методом POST

# Авторизация

> Как передавать токен авторизации в запросах

```shell
curl "https://selsup.ru/<host>/api/product/findProduct" \
  -H "Authorization: <token>"
```

> Проверьте, что у вас указан ваш базовый адрес сервера вместо &lt;host&gt; и &lt;token&gt; заменен на ваш токен API. Они указаны на странице добавления нового токена

Перейдите на страницу настройки API:
<a href='https://selsup.ru/application/integration/pageApi'>https://selsup.ru/application/integration/pageApi</a>

Введите название нового токена в поле и нажмите кнопку Добавить токен. Название должно быть уникальным в рамках вашего
аккаунта. Рекомендуется для разных сервисов использовать свои токены, чтобы в любой момент можно было отозвать токен.

Токен необходимо передавать в заголовке Authorization: <токен>

Адрес для запросов API так же указан на странице выше в тексте: "Используйте базовый адрес, вашего сервера: ..."

Например: https://selsup.ru/<ваш сервер>/api/

# Товары

## Поиск товаров

```shell
curl "https://selsup.ru/<host>/api/product/findProduct?query=123&&count=true&sortBy=ID" \
  -H "Authorization: <token>"
```

> В результате отдается JSON товаров

```json
{
  "page": 1,
  "total": 1000,
  "rows": [
    {
      "id": 1,
      "name": "Название товара",
      "deleted": false,
      "skuId": 1234,
      "size": "52",
      "vendorSize": "XL",
      "organizationId": 1,
      "ozonArticle": "Артикул Ozon",
      "aliexpressSku": "Артикул Aliexpress",
      "sberArticle": "Артикул СберМегаМаркета",
      "yandexMarketShopSku": "Артикул Яндекс.Маркета",
      "view": {
        "id": 1,
        "color": "Название цвета или пусто",
        "wbArticle": "Артикул Яндекс.Маркета",
        "model": {
          "id": 1,
          "manufacturer": {
            "id": 1,
            "name": "Производитель"
          },
          "category": {
            "categoryId": 1,
            "name": "Название категории"
          },
          "brand": {
            "brandId": 1,
            "name": "Название бренда"
          }
        }
      }
    }
  ]
}
```

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/findProduct">Полный список полей</a>

Позволяет найти товары по фильтрам и поисковому запросу, либо просто получить все товары по порядку. Для выбора всех товаров лучше передавать sortBy=ID, чтобы новые товары не изменяли порядок сортировки. count=true лучше передавать только в первом запросе. Метод не отдает полную информацию о товаре, только основные поля, которые отображаются на списке товаров. Чтобы получить полную информацию о карточке, необходимо запросить ее по ID модели.

### Параметры запроса

Параметр | Тип | Описание
--------- | ------- | -----------
limit | int32 | Количество заказов на странице, по умолчанию - 100, максимум 500
page | int32 | Номер страницы, начиная с 1
count | boolean | Возвратить в ответе общее количество записей
sortBy | enum | Поле по которому отсортировать данные. Возможные значения: "ID" - идентификатор товаров, "NAME" - название товара, "TOTALORDERS" - общее количество заказов;
ascending | boolean | Сортировать по возврастанию данные
query | string | Поисковый запрос по названию, штрих-коду или артикулу товара
article | string | Поиск по артикула товара в SelSup
needToBuy | boolean | Товары, которые необходимо закупить
inStock | boolean | Наличие остатков товаров на маркетплейсе или складе SelSup
noStock | boolean | Отсутствие остатков товаров на маркетплейсе и складе SelSup

### Тело ответа

Поле | Тип | Описание
--------- | ------- | -----------
page | int32 | Номер запрошенной страницы
total | int32 | Общее количество заказов по фильтру. Отдается только если задан count=true
rows | array of (Product) | Товары

### Структура Product

Поле | Тип | Описание
--------- | ------- | -----------
id | int64 | Идентификатор товара
name | string | Название товара
skuId | int64 | Номер SKU товара для хранения остатков
createdDate | string | Дата создания товара
createdUser | string | Логин пользователя, который создал товар
ozonArticle | string | Артикул на маркетплейсе Ozon
yandexMarketShopSku | string | Артикул на маркетплейсе Яндекс.Маркет
price | double | Цена со скидкой
priceWithoutDiscount | double | Цена без скидки
purchasePrice | double | Закупочная цена товара
wildberriesPrice | double | Текущая цена на Wildberries
ozonPrice | double | Текущая цена на Ozon
sberMegaMarketPrice | double | Текущая цена на СберМегаМаркет
yandexMarketPrice | double | Текущая цена на Яндекс.Маркет
aliexpressPrice | double | Текущая цена на Aliexpress
view | ProductView | Объект описывающий Цвет товара
barcodes | array of ProductBarcode | Список штрих-кодов

### Структура ProductBarcode

Поле | Тип | Описание
--------- | ------- | -----------
id | int64 | Идентификатор штрих-кода
barcode | string | Штрих-код
useInWildberries | boolean | Используется в Wildberries
useInOzon | boolean | Используется в Ozon
useInYandexMarket | boolean | Используется в Яндекс.Маркет

### Структура ProductView

Поле | Тип | Описание
--------- | ------- | -----------
id | int64 | Идентификатор цвета
color | string | Название цвета товара
model | ProductModel | Объект описывающий Модель товара

### Структура ProductModel

Поле | Тип | Описание
--------- | ------- | -----------
id | int64 | Идентификатор модели
name | string | Название модели
brand | Brand | Объект описывающий Бренд товара
category | Category | Объект описывающий Категорию товара

## Информация о карточке

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/getModelById">Полный список полей</a>

```shell
curl "https://selsup.ru/<host>/api/product/getModelById?id=1" \
  -H "Authorization: <token>"
```

> В результате отдается JSON товаров

```json
{
  "id": 1,
  "article": "Уникальный артикул модели",
  "name": "Название модели",
  "title": "Название модели для печати и автоматического формирования названий товаров",
  "description": "Описание",
  "site": "https://example.ru/",
  "services": ["WILDBERRIES"],
  "manufacturer": {
    "id": 1,
    "name": "Производитель"
  },
  "category": {
    "categoryId": 1,
    "name": "Название категории"
  },
  "brand": {
    "brandId": 1,
    "name": "Название бренда"
  },
  "views": [
    {
      "id": 1,
      "color": "Название цвета",
      "sizes": [
        {
          "id": 1,
          "name": "Название товара",
          "size": "Размер или параметры",
          "realSize": "Российский размер",
          "vendorSize": "Размер производителя"
        }
      ]
    }
  ]
}
```

Позволяет получить всю информацию о карточке товара, включая все заполненные параметры для последующего изменения информации о товаре через метод updateModel. В списке services передаются маркетплейсы или сервисы, в которые необходимо отправить карточку после сохранения.

## Создание модели

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/createModel">Полный список полей</a>

```shell
curl -X POST "https://selsup.ru/<host>/api/product/createModel" \
  -H "Authorization: <token>" -data '{
  "article": "Уникальный артикул товара",
  "organizationId": 123,
  "categoryId": 123,
  "brandId": 123,
  "manufacturerId": 123,
  "views": [
    {
      "color": "Название цвета",
      "sizes": [
        {
          "name": "Название товара",
          "barcodes": [
            {
              "barcode": "штрих-код товара"
            }
          ]        
        }
      ]
    }
  ]
}'
```

> В результате отдается JSON модели, с проставленным значение id у модели, цвета и размера

```json
{
  "id": 1,
  "article": "Артикул модели",
  "createdDate": "Дата создания заказа",
  "views": [
    {
      "id": 1,
      "color": "Название цвета",
      "sizes": [
        {
          "id": 1,
          "size": "Размер",
          "name": "Название товара"
        }
      ]
    }
  ]
}
```

Позволяет создавать карточку товара. Карточка состоит из нескольких цветов, у каждого цвета может быть несколько размеров. Если в вашей категории товаров нет разделения по цветам и размерам - то просто создается карточка с одним цветом и одним размером. Цвет указывать не обязательно, как и значения поля размера. Обязательно указывать только артикул у модели.

Штрих-коды если не указаны, будут автоматически сгенерированы SelSup, либо нужно указывать явно штрих-коды в карточке товара. 

### Тело запроса

Поле | Тип | Обязательность | Описание
--------- | ------- | ------- | -----------
id | int64 | Нет | Идентификатор модели, передается только при редактировании
article | string | Да | Артикул модели
manufacturerId | int64 | Да | Идентификатор производителя - если не указано используется производитель по умолчанию у организации
brandId | int64 | Да | Идентификатор бренда - если не указано используется бренд по умолчанию у организации
categoryId | int64 | Да | Идентификатор категории из ответа knowledge/findCategory
views | array of ProductView | Да | Список цветов
countryName | string | Нет | Название страны производства товара
materials | string | Нет | Состав
packWidth | int64 | Нет | Ширина упаковки товара в мм
packHeight | int64 | Нет | Ширина упаковки товара в мм
name | string | Нет | Название модели
description | string | Нет | Описание товара, используется если не заполнен параметр описания для конкретного маркетплейса
services | array of Service | Нет | Список маркетплейсов или сервисов, в которые можно отправить товар после сохранения

### Структура ProductView

Поле | Тип | Обязательность | Описание
--------- | ------- | ------- | -----------
id | int64 | Нет | Идентификатор цвета, только при редактировании
color | string | Нет | Название цвета
wbArticle | string | Нет | Артикул карточки Wildberries
sizes | array of Product | Да | Список размеров

### Структура Product

Поле | Тип | Обязательность | Описание
--------- | ------- | ------- | -----------
id | int64 | Нет | Идентификатор товара, только при редактировании
size | string | Нет | Размер товара или параметры
name | string | Да | Название товара полное

## Редактирование модели

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/updateModel">Полный список полей</a>

```shell
curl -X POST "https://selsup.ru/<host>/api/product/updateModel" \
  -H "Authorization: <token>" -data '{
  "article": "Уникальный артикул товара",
  "organizationId": 123,
  "categoryId": 123,
  "brandId": 123,
  "manufacturerId": 123,
  "views": [
    {
      "color": "Название цвета",
      "sizes": [
        {
          "name": "Название товара",
          "barcodes": [
            {
              "barcode": "штрих-код товара"
            }
          ]        
        }
      ]
    }
  ]
}'
```

> В результате отдается JSON модели, с проставленным значение id у модели, цвета и размера

```json
{
  "id": 1,
  "article": "Артикул модели",
  "createdDate": "Дата создания заказа",
  "views": [
    {
      "id": 1,
      "color": "Название цвета",
      "sizes": [
        {
          "id": 1,
          "size": "Размер",
          "name": "Название товара"
        }
      ]
    }
  ]
}
```

Позволяет изменить информацию у существующей модели, добавить цвета или размеры. Так же при редактировании модели можно выставлять свойство hasChanges=false, чтобы не изменять некоторые цвета или размеры.

## Поиск категорий

<a href="https://api.selsup.ru/all.html#tag/Znaniya-o-tovarah/operation/findCategory">Полный список полей</a>

```shell
curl "https://selsup.ru/<host>/api/knowledge/findCategory?query=text&limit=50" \
  -H "Authorization: <token>"
```

> В результате отдается JSON со списком категорий

```json
{
  "rows": [
    {
      "categoryId": 0,
      "name": "Название категории",
      "deleted": true,
      "parentId": 0,
      "marked": true,
      "categoryClass": "SHOES",
      "wildberriesType": {
        "id": 0,
        "name": "Категория Wildberries"
      },
      "ozonCategory": {
        "id": 0,
        "name": "Категория Ozon"
      },
      "tnved": {
        "id": 0,
        "name": "ТНВЭД"
      },
      "aliexpressCategory": {
        "id": 0,
        "name": "Категория Aliexpress"
      },
      "avitoCategory": {
        "id": 0,
        "name": "Категория Авито"
      }
    }
  ],
  "total": 0,
  "page": 0
}
```

Позволяет найти категории по запросу или фильтру. Категории SelSup могут связываться с категориями маркетплейсов, могут хранить параметры, которые автоматически проставляются в карточках при создании, если параметр не заполнен в модели. Идентификатор категории необходимо использовать для создания товара.

### Параметры запроса

Параметр | Тип | Описание
--------- | ------- | -----------
limit | int32 | Количество категорий на странице, по умолчанию - 100, максимум 500
page | int32 | Номер страницы, начиная с 1
count | boolean | Возвратить в ответе общее количество записей
sortBy | enum | Поле по которому отсортировать данные
ascending | boolean | Сортировать по возврастанию данные
query | string | Поисковый запрос по названию, штрих-коду или артикулу товара

### Тело ответа

Поле | Тип | Описание
--------- | ------- | -----------
page | int32 | Номер запрошенной страницы
total | int32 | Общее количество заказов по фильтру. Отдается только если задан count=true
rows | array of (Category) | Категории

### Структура Category

Поле | Тип | Описание
--------- | ------- | -----------
categoryId | int64 | Идентификатор категории
name | string | Название категории
marked | boolean | Подлежат ли товары этой категории маркировке через Честный знак
hasSize | boolean | Разделяются ли товары данной категории по размерам
hasColor | boolean | Разделяются ли товары данной категории по цветам
autoName | boolean | Автоматически формировать название для товаров из данной категории по шаблону
namePattern | string | Шаблон названия товаров в данной категории
removeFbsStock | boolean | Убрать остатки FBS для товаров из данной категории
wildberriesType | WildberriesType | Информация о категории Wildberries
categoryClass | enum | Класс категории для получения маркировки Честного знака. Одно из значений: "SHOES" "LIGHT" "DRUGS" "CAMERA" "TYRES" "MILK" "WATER" "TOBACCO" "FURS" "BEER" "BICYCLES" "PERFUMES" "ELECTRONIC" "OTHER" "NONE" "FOOD" "WHEELCHAIRS" "OTP" "NCP" "BIO" "ANTISEPTIC" "NABEER" "SOFTDRINKS"
deleted | boolean | Помечен удаленным?

## Поиск бренда

<a href="https://api.selsup.ru/all.html#tag/Znaniya-o-tovarah/operation/findBrand">Полный список полей</a>

```shell
curl "https://selsup.ru/<host>/api/knowledge/findBrand?query=text&limit=50" \
  -H "Authorization: <token>"
```

> В результате отдается JSON со списком категорий

```json
{
  "rows": [
    {
      "brandId": 0,
      "name": "string",
      "logo": "Путь к логотипу",
      "logoSize": 0,
      "logoWidth": 0,
      "logoHeight": 0,
      "deleted": true,
      "ozonName": "Название бренда на Ozon",
      "ozonId": 0
    }
  ],
  "total": 0,
  "page": 0
}
```

Позволяет найти бренды по запросу или фильтру. Идентификатор бренда необходимо использовать для создания товара.

### Параметры запроса

Параметр | Тип | Описание
--------- | ------- | -----------
limit | int32 | Количество брендов на странице, по умолчанию - 100, максимум 500
page | int32 | Номер страницы, начиная с 1
count | boolean | Возвратить в ответе общее количество записей
sortBy | enum | Поле по которому отсортировать данные
ascending | boolean | Сортировать по возврастанию данные
query | string | Поисковый запрос по названию, штрих-коду или артикулу товара

### Тело ответа

Поле | Тип | Описание
--------- | ------- | -----------
page | int32 | Номер запрошенной страницы
total | int32 | Общее количество заказов по фильтру. Отдается только если задан count=true
rows | array of (Brand) | Бренды

### Структура Brand

Поле | Тип | Описание
--------- | ------- | -----------
brandId | int64 | Идентификатор бренда
name | string | Название бренда
ozonId | int64 | Идентификатор бренда на Ozon
ozonName | string | Название бренда на Ozon
deleted | boolean | Помечен удаленным?

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

Вы можете передавать новые заказы в SelSup по API, например с вашего интернет-магазина. В запросе необходимо передавать
информацию о заказе и список товаров в заказе. В качестве уникального ключа, для того, чтобы не создавать дубликаты
заказов используйте externalOrderId - номер заказа на сайте интернет-магазина.

Поле organizationId нужно обязательно передавать, если у вас в аккаунте несколько организаций.

Для передачи товаров необходимо предварительно связать товары сайта, с товарами в SelSup, чтобы потом передавать
productId - идентификатор товара в SelSup. Сделать это можно разовов, импортировав все товары методом findProduct

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

Вы можете передавать новые заказы в SelSup по API, например с вашего интернет-магазина. В запросе необходимо передавать
информацию о заказе и список товаров в заказе. В качестве уникального ключа, для того, чтобы не создавать дубликаты
используется externalOrderId

### Запрос

`POST https://selsup.ru/<host>/api/order/createOrder`

### Тело запроса JSON с ключами

Поле  | Тип | Обязательный | Описание
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

В случае ошибки отдается код ответа 400, а в теле отдается message - с кодом ошибки и messageParams - дополнительные
параметры сообщения об ошибке. Любые коды, отличные от 200 - ошибка запроса

Сообщение | Причина
--------- | -------
error_empty_warehouse | Не указан параметр warehouseId для заказа на маркетплейс type=FBM
error_no_organization | Не указано поле organizationId, если в аккаунте несколько организаций
error_no_marketplace | Не указан service, для type=FBS или type=FBM заказов
error_no_quantity_for_order_product | Не указано количество товара у позиции заказа products[index].quantity
error_no_price_for_order_product | Не указана цена у позиции заказа products[index].price

## Получение списка заказов

```shell
curl "https://selsup.ru/<host>/api/order/findOrder?type=FBS&actual=true&count=true" \
  -H "Authorization: <token>"
```

> В результате отдается JSON заказов

```json
{
  "page": 1,
  "total": 1000,
  "rows": [
    {
      "id": 0,
      "name": "string",
      "type": "FBM",
      "boxType": "DEFAULT",
      "service": "NONE",
      "writeOffStocks": true,
      "warehouseId": 0,
      "stockWarehouseId": 0,
      "deliveryDate": "2019-08-24T14:15:22Z",
      "invoiceNumber": 0,
      "externalOrderId": "string",
      "organizationId": 0,
      "products": [
        {
          "id": 0,
          "product": {
            "id": 0,
            "productType": "PRODUCT",
            "name": "string",
            "printName": "string",
            "realSize": "string",
            "vendorSize": "string",
            "size": "string",
            "ozonArticle": "string",
            "removeFbsStock": true,
            "purchaseCurrency": "RUB",
            "versionId": 0,
            "sourceIncomeItemId": 0,
            "organizationId": 0,
            "suzId": "string",
            "barcodes": [
              {
                "id": 0,
                "barcode": "string",
                "organizationId": 0,
                "clientId": 0,
                "productId": 0,
                "useInWildberries": true,
                "useInOzon": true,
                "useInYandexMarket": true,
                "useInAliexpress": true,
                "format": "string"
              }
            ]
          },
          "quantity": 0,
          "price": 0,
          "otherExpenses": 0,
          "wholesalePrice": 0,
          "position": 0,
          "cell": {
            "id": 0,
            "fullName": "string"
          },
          "createdDate": "2019-08-24T14:15:22Z",
          "createdUser": "string",
          "error": "string",
          "lastUpdateDate": "2019-08-24T14:15:22Z"
        }
      ],
      "supply": {
        "id": 0,
        "service": "NONE",
        "name": "string",
        "type": "FBM",
        "boxType": "DEFAULT",
        "status": "CREATED",
        "createdDate": "2019-08-24T14:15:22Z",
        "createdUser": "string",
        "warehouseId": 0,
        "deliveryDate": "2019-08-24T14:15:22Z",
        "clientId": 0,
        "organizationId": 0,
        "deleted": true,
        "externalId": "string",
        "deliveryMethod": 0,
        "deliveryMethodName": 0,
        "versionId": 0,
        "orderId": 0,
        "supplyId": 0,
        "supplyName": {}
      },
      "comment": "string",
      "gtd": "string",
      "serviceWarehouseId": "string",
      "deliveryRegion": "string",
      "logistics": 0,
      "totalCommissions": 0,
      "saleDate": "2019-08-24T14:15:22Z",
      "returnDate": "2019-08-24T14:15:22Z",
      "deliveryService": "CDEK",
      "deliveryServiceIdentification": "string",
      "deliveryPoint": "string",
      "deliveryPointCode": "string",
      "deliveryLabelPath": "string",
      "deliveryLabelRequest": "string",
      "edoId": "string",
      "orderEdoId": {},
      "stickerId": 0,
      "stickerText": "string"
    }
  ]
}
```

Позволяет получить список заказов с фильтрацией и поиском. В данном методе содержимое заказа не отдается сразу, чтобы
получить заказы сразу с товарами, используйте другой метод /api/fbs/findOrder. Если нужно получить общее количество -
необходимо обязательно проставить count=true. Лучше count=true ставить только в первом запросе - далее не передавать

### Параметры запроса

Параметр | Тип | Описание
--------- | ------- | -----------
limit | int32 | Количество заказов на странице, по умолчанию - 100, максимум 500
page | int32 | Номер страницы, начиная с 1
count | boolean | Возвратить в ответе общее количество записей
sortBy | enum | Поле по которому отсортировать данные. Возможные значения: "ID" - идентификатору заказа, лучше всего по номеру сортировать по умолчанию. "CREATEDDATE" - дата создания заказа на маркетплейсе. "CREATED" - дата создания заказа в SelSup. "CREATEDUSER" - пользователь, который создал заказ. "STATUS" - статус заказа. "SERVICE" - маркетплейс заказа. "TYPE" - тип заказа. "BOXTYPE" - тип короба заказа, "NAME" - название заказа, "ORGANIZATIONID" - идентификатор организации, "WAREHOUSE" - склад заказа, "INVOICENUMBER" - номер счета заказа, "EXTERNALORDERID" - внешний номер заказа, "QUEUE" - очередь заказов на сборку, "SUPPLYID" - идентификатору поставки, "CLOSEDATE" "CLOSEUSERID" "USERID" "PRICE" "PRODUCT" "COLLECTORDERTASKID" "DELIVERYDATE" "MARKINGSTATUS"
ascending | boolean | Сортировать по возврастанию данные
query | string | Поисковый запрос по номеру заказа
actual | boolean | Отображать только актуальные заказы - не закрытые
type | enum | Тип заказа строкой: "FBM" - отгрузка на маркетплейс, "FBS" - заказ с маркетплейса со склада продавца, "FBO" - заказ с маркетплейса со склада самого маркетплейса, "RETAIL" - розничный заказ, в том числе с сайта, "WHOLESALE" - оптовый заказ
organizations | Array of integers <int64> uniq | Оргиназции по которым отдать заказы
service | enum | Сервис или маркетплейс заказа: "NONE", "WILDBERRIES", "OZON", "YANDEX_MARKET", "FAMILIYA", "NATIONAL_CATALOG", "ALIEXPRESS", "OTHER", "MOY_SKLAD", "SBER_MEGA_MARKET", "CISLINK" "ONE_C", "AVITO", "LEROY_MERLIN", "DETMIR", "KAZAN_EXPRESS"
productId | int64 | Идентификатор товара в SelSup, который есть в заказе
status | enum | Фильтр по статусу заказа "CREATED" - Новые заказ, "REVOKING" - Отменяется на маркетплейсе, "REVOKED" - Отменен на маркетплейсе, "ORDER_CREATED" - в основном для FBM - создан на маркетплейсе, "CARDS_CREATED" - в основном для FBM - карточки созданы, "BOX_BARCODES" - в основном для FBM, загрузка ШК коробов, "IMAGES_UPLOADED" - в основном FBM - изображения товаров загружены, "INVOICE_UPLOADED" - в основном для FBM - счет загружен, "READY_TO_SUPPLY" - в основном для FBM - готов к отгрузке, "SUPPLIED" - в основном для FBM - отгружен, "FINISHED" - завершен, "COLLECTED" - собран, "SEND" - отправлен, "DELIVERY" - доставляется, "REFUND" - возврат, "PACKING" - на сборке, "PAYMENT_REQUIRED" - ожидает оплаты, "CANCELED" - отменен покупателем, "CONTROVERSIAL" - спорный, "OVERDUE" - просрочен, "READY_FOR_PICKUP" - готов к выдаче на пункте доставки
noLabel | boolean | Фильтр по заказам без этикетки у заказа для печати
dbs | boolean | Фильтр по заказам DBS
supplyId | int64 | Фильтр по номеру поставки SelSup

### Тело ответа

Поле | Тип | Описание
--------- | ------- | -----------
page | int32 | Номер запрошенной страницы
total | int32 | Общее количество заказов по фильтру. Отдается только если задан count=true
rows | array of (Order) | Заказы

# Остатки

## Остатки в SelSup

Остатки товаров в SelSup привязываются к SKU - единице хранения на складе. Каждому товару присваивается свой номер SKU и в дальнейшем можно указать одинаковый SKU для нескольких разных товаров в SelSup.

Вы можете использовать две схемы хранения остатков в SelSup:
1)Когда на каждую штуку товара клеится отдельный уникальный код, по которому можно отслеживать всю историю товара и вы всегда можете отделить каждую единицу товара друг от друга. Данный уникальный стикер позволяет вам клеить его в удобное для быстрого поиска место товара, что существенно ускоряет сборку товаров на складе и их идентификацию - особенно если вы работаете с кодами маркировки честного знака
2)Когда остаток хранится просто к привязки к ячейке по штрих-коду. В этом случае в остатке записывается количество - сколько лежит определенного товара в данной ячейке.

## Изменение остатков

<a href="https://api.selsup.ru/all.html#tag/Hranenie-tovara-na-sklade-(WMS)/operation/changeStock">Полный список полей</a>

```shell
curl -X POST "https://selsup.ru/<host>/api/wms/changeStock?skuId=123&stock=5&warehouseId=123" \
  -H "Authorization: <token>"
```

> В результате отдается 200 код ответа или 400 в случае ошибки

Позволяет для SKU изменить остатки товаров на складе. Работает для всех схем хранения, как с уникальными кодами, так и без них
