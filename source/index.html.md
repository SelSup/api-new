---
title: Документация API SelSup

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - shell

toc_footers:
  - <a href='https://selsup.ru/application/integration/api'>Добавить ключ API</a>
  - <a href='https://api.selsup.ru/all.html'>Список всех методов</a>
  - <a href='https://t.me/+fPI_QY47oG0xOGUy'>Чат разработчиков API</a>

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
Aliexpress, Яндекс.Маркет, СберМегаМаркет, Леруа Мерлен, кассами Эвотор и Авито. Работать со службами доставки.

SelSup позволяет создавать карточки на всех маркетплейсах, заполнять параметры, вести учет остатков товаров, принимать
заказы по FBS с маркетплейсов и интернет-магазина, обновлять остатки на позиции, по которым пришел заказ. Вести
аналитику продаж и учет финансов.

С помощью API вы можете подключить любые источники заказов к SelSup и вести быстрый учет остатков с маркетплейсов, сайта
и других источников заказов.

Вы можете посмотреть полный список методов по ссылке:
<a href='https://api.selsup.ru/all.html'>https://api.selsup.ru/all.html</a>

По умолчанию GET запросы используются для получения данных, все запросы на изменение данных отправляются методом POST

# Авторизация

> Как передавать токен авторизации в запросах

```shell
curl "https://api.selsup.ru/api/product/findProduct" \
  -H "Authorization: <token>"
```

> Проверьте, что у вас указан токен API вместо &lt;token&gt;. Он указан на странице добавления нового токена

Перейдите на страницу настройки API:
<a href='https://selsup.ru/application/integration/api'>https://selsup.ru/application/integration/api</a>

Введите название нового токена в поле и нажмите кнопку Добавить токен. Название должно быть уникальным в рамках вашего
аккаунта. Рекомендуется для разных сервисов использовать свои токены, чтобы в любой момент можно было отозвать токен.

Токен необходимо передавать в заголовке Authorization: <токен>

# Webhooks

SelSup может отправлять запросы на внешние сервисы и так же получать информацию от внешних сервисов, в случае каких-то событий. Такими событиями могут быть:
 - получение заказа с маркетплейса или от покупателя, в этом случае передается полная информация о заказе
 - изменение остатка на товары, передается информация о товаре и новый остаток
 - изменение цены на товар

Вы можете настроить Webhook, который будет отправлять HTTP/HTTPS запрос на адрес, который будет указан в кабинете. Возможна отправка POST или GET запроса в нужном формате с необходимыми заголовками.

Так вы сможете настроить интеграцию с любой собственной системой или сайтом.

# Разработка приложений для SelSup

Вы можете создавать различные приложения для SelSup, которые будут расширять возможности SelSup и добавлять новые функции.

# Загрузка файлов

```shell
curl "https://api.selsup.ru/api/files/view?path=<path>" \
  -H "Authorization: <token>"
```

Все файлы - изображения, этикетки, документы, видео SelSup сохраняет в файловое хранилище. Все этикетки в этом хранилище отдаются по пути до файла - path. Запросить данные из хранилища вы можете через метод /api/files/view?path=<путь до файла>, при этом для получения некоторых файлов требуется авторизация, а некоторые, как картинки, могут отдаваться и без авторизации.

В случае отсутствия файла отдается ошибка с кодом ответа 404

Вы можете использовать метод /api/files/file?path=<путь до файла>, если вы хотите дать ссылку для скачивания файла

# Товары

## Поиск товаров

```shell
curl "https://api.selsup.ru/api/product/findProduct?query=123&&count=true&sortBy=ID" \
  -H "Authorization: <token>"
```

> В результате отдается JSON товаров

```json
{
  "page": 1,
  "total": 1000,
  "rows": [
    {
      "id": 6510,
      "productType": "PRODUCT",
      "name": "Платья D0606черный черный, размер 44-46",
      "deleted": false,
      "printName": "Платья D0606черный черный, размер 44-46",
      "realSize": "44-46",
      "vendorSize": "44-46",
      "size": "44-46",
      "wildberriesStockCount": 0,
      "wildberriesSizeId": 141561601,
      "ozonStockCount": 0,
      "ozonStockBetweenWarehouses": 0,
      "removeFbsStock": false,
      "purchaseCurrency": "RUB",
      "createdDate": "2023-06-06T17:55:24Z[UTC]",
      "createdUser": "Импорт",
      "wildberriesOrderQuantity": 2,
      "wildberriesSupplyingQuantity": 0,
      "wildberriesSaleQuantity": 0,
      "wildberriesQuantityInWay": 1,
      "lastStockChange": "2023-10-16T13:30:06Z[UTC]",
      "ozonOrderQuantity": 0,
      "ozonSupplyingQuantity": 0,
      "instockQuantity": 0,
      "clientId": 1,
      "organizationId": 1,
      "productViewId": 4834,
      "view": {
        "color": "D0606черный",
        "wbArticle": "D0606черный",
        "id": 4834,
        "model": {
          "article": "68996302",
          "category": {
            "categoryId": 36,
            "name": "Инструменты",
            "marked": true,
            "categoryClass": "OTHER",
            "tnved": {
              "id": 6204430000,
              "name": null,
              "deleted": null,
              "type": "MAIN",
              "categoryClass": null,
              "parentId": null,
              "keywords": null,
              "description": null,
              "paramsTnvedId": null,
              "isInParam": null,
              "certification": null,
              "code": "6204430000",
              "parentCode": ""
            },
            "tnvedId": 6204430000,
            "removeFbsStock": false
          },
          "manufacturer": {
            "manufacturerId": 1,
            "title": "Default",
            "name": "Default",
            "address": "Россия"
          },
          "brand": {
            "brandId": 10,
            "name": "Monterey"
          },
          "brandId": 10,
          "id": 4302,
          "name": "Платье летнее женское больших размеров хлопок повседневное",
          "title": "Платье летнее женское больших размеров хлопок повседневное",
          "materials": "хлопок 50%, вискоза 30%, полиэстер 20%",
          "deleted": false,
          "vat": "NONE",
          "countryId": 643,
          "countryName": "Россия",
          "favourite": false
        },
        "actual": true,
        "mainImage": {
          "type": null,
          "id": 54,
          "path": "product/images/4834/4eac5720-99ba-4cbc-ab52-c6351ce02977.jpg",
          "position": 1,
          "width": 900,
          "height": 1200,
          "productViewId": 4834,
          "size": 525879,
          "wildberriesImgUUID": null,
          "services": null
        },
        "mainImageId": 54,
        "mainImageUrl": "https://basket-05.wb.ru/vol870/part87042/87042014/images/big/1.jpg",
        "wildberriesId": 87042014,
        "deleted": false
      },
      "wildberriesStatus": "SUCCESS",
      "nationalCatalogStatus": "UNKNOWN",
      "ozonStatus": "UNKNOWN",
      "wildberriesFbsOrdersQuantity": 0,
      "ozonFbsOrdersQuantity": 0,
      "ymarketFbsOrdersQuantity": 0,
      "skuId": 6121,
      "barcodes": [
        {
          "id": 6073,
          "barcode": "4605505279171",
          "organizationId": 0,
          "clientId": 1,
          "productId": 6510,
          "useInWildberries": true,
          "useInOzon": true,
          "useInYandexMarket": true,
          "useInAliexpress": false,
          "format": "ean13"
        }
      ],
      "group": {
        "48-50": {
          "id": 6511,
          "productType": "PRODUCT",
          "name": "Платья D0606черный черный, размер 48-50",
          "deleted": false,
          "printName": "Платья D0606черный черный, размер 48-50",
          "realSize": "48-50",
          "vendorSize": "48-50",
          "size": "48-50",
          "wildberriesStockCount": 0,
          "wildberriesSizeId": 141561603,
          "ozonStockCount": 0,
          "ozonStockBetweenWarehouses": 0,
          "removeFbsStock": false,
          "purchaseCurrency": "RUB",
          "createdDate": "2023-06-06T17:55:24Z[UTC]",
          "createdUser": "Импорт",
          "wildberriesOrderQuantity": 0,
          "wildberriesSupplyingQuantity": 0,
          "wildberriesSaleQuantity": 0,
          "wildberriesQuantityInWay": 0,
          "ozonOrderQuantity": 0,
          "ozonSupplyingQuantity": 0,
          "instockQuantity": 0,
          "clientId": 1,
          "organizationId": 1,
          "productViewId": 4834,
          "wildberriesStatus": "SUCCESS",
          "nationalCatalogStatus": "UNKNOWN",
          "ozonStatus": "UNKNOWN",
          "wildberriesFbsOrdersQuantity": 0,
          "ozonFbsOrdersQuantity": 0,
          "ymarketFbsOrdersQuantity": 0,
          "skuId": 6122,
          "barcodes": [
            {
              "id": 6074,
              "barcode": "4605505279195",
              "organizationId": 0,
              "clientId": 1,
              "productId": 6511,
              "useInWildberries": true,
              "useInOzon": true,
              "useInYandexMarket": true,
              "useInAliexpress": false,
              "format": "ean13"
            }
          ],
          "yandexMarketStatus": "UNKNOWN",
          "aliexpressStatus": "UNKNOWN",
          "moySkladStatus": "UNKNOWN",
          "avitoStatus": "UNKNOWN",
          "removeFbsStockOzon": false,
          "removeFbsStockWb": false,
          "removeFbsStockAli": false,
          "removeFbsStockYm": false,
          "removeFbsStockSber": false,
          "price": 630.0000000000001,
          "priceWithoutDiscount": 4200,
          "deliveryCost": 0,
          "wildberriesPrice": 630.0000000000001,
          "wildberriesPriceWithoutDiscount": 4200,
          "salesExpensesOnMpPercent": 20,
          "taxeRate": 0,
          "desiredMarginalityPercent": 0,
          "desiredProfitRub": 0,
          "additionalCost": 0,
          "packWidth": 250,
          "packHeight": 450,
          "totalOrdersCount": 0,
          "totalFbsOrdersCount": 0,
          "emptyBarcodes": false
        },
        "46-48": {
          "id": 6513,
          "productType": "PRODUCT",
          "name": "Платья D0606черный черный, размер 46-48",
          "deleted": false,
          "printName": "Платья D0606черный черный, размер 46-48",
          "realSize": "46-48",
          "vendorSize": "46-48",
          "size": "46-48",
          "wildberriesStockCount": 0,
          "wildberriesSizeId": 141561602,
          "ozonStockCount": 0,
          "ozonStockBetweenWarehouses": 0,
          "removeFbsStock": false,
          "purchaseCurrency": "RUB",
          "createdDate": "2023-06-06T17:55:25Z[UTC]",
          "createdUser": "Импорт",
          "wildberriesOrderQuantity": 0,
          "wildberriesSupplyingQuantity": 0,
          "wildberriesSaleQuantity": 0,
          "wildberriesQuantityInWay": 0,
          "ozonOrderQuantity": 0,
          "ozonSupplyingQuantity": 0,
          "instockQuantity": 0,
          "clientId": 1,
          "organizationId": 1,
          "productViewId": 4834,
          "wildberriesStatus": "SUCCESS",
          "nationalCatalogStatus": "UNKNOWN",
          "ozonStatus": "UNKNOWN",
          "wildberriesFbsOrdersQuantity": 0,
          "ozonFbsOrdersQuantity": 0,
          "ymarketFbsOrdersQuantity": 0,
          "skuId": 6124,
          "barcodes": [
            {
              "id": 6076,
              "barcode": "4605505279188",
              "organizationId": 0,
              "clientId": 1,
              "productId": 6513,
              "useInWildberries": true,
              "useInOzon": true,
              "useInYandexMarket": true,
              "useInAliexpress": false,
              "format": "ean13"
            }
          ],
          "yandexMarketStatus": "UNKNOWN",
          "aliexpressStatus": "UNKNOWN",
          "moySkladStatus": "UNKNOWN",
          "avitoStatus": "UNKNOWN",
          "removeFbsStockOzon": false,
          "removeFbsStockWb": false,
          "removeFbsStockAli": false,
          "removeFbsStockYm": false,
          "removeFbsStockSber": false,
          "price": 630.0000000000001,
          "priceWithoutDiscount": 4200,
          "deliveryCost": 0,
          "wildberriesPrice": 630.0000000000001,
          "wildberriesPriceWithoutDiscount": 4200,
          "salesExpensesOnMpPercent": 20,
          "taxeRate": 0,
          "desiredMarginalityPercent": 0,
          "desiredProfitRub": 0,
          "additionalCost": 0,
          "packWidth": 250,
          "packHeight": 450,
          "totalOrdersCount": 0,
          "totalFbsOrdersCount": 0,
          "emptyBarcodes": false
        },
        "50-52": {
          "id": 6512,
          "productType": "PRODUCT",
          "name": "Платья D0606черный черный, размер 50-52",
          "deleted": false,
          "printName": "Платья D0606черный черный, размер 50-52",
          "realSize": "50-52",
          "vendorSize": "50-52",
          "size": "50-52",
          "wildberriesStockCount": 0,
          "wildberriesSizeId": 141561604,
          "ozonStockCount": 0,
          "ozonStockBetweenWarehouses": 0,
          "removeFbsStock": false,
          "purchaseCurrency": "RUB",
          "createdDate": "2023-06-06T17:55:24Z[UTC]",
          "createdUser": "Импорт",
          "wildberriesOrderQuantity": 0,
          "wildberriesSupplyingQuantity": 0,
          "wildberriesSaleQuantity": 0,
          "wildberriesQuantityInWay": 0,
          "ozonOrderQuantity": 0,
          "ozonSupplyingQuantity": 0,
          "instockQuantity": 0,
          "clientId": 1,
          "organizationId": 1,
          "productViewId": 4834,
          "wildberriesStatus": "SUCCESS",
          "nationalCatalogStatus": "UNKNOWN",
          "ozonStatus": "UNKNOWN",
          "wildberriesFbsOrdersQuantity": 0,
          "ozonFbsOrdersQuantity": 0,
          "ymarketFbsOrdersQuantity": 0,
          "skuId": 6123,
          "barcodes": [
            {
              "id": 6075,
              "barcode": "4605505279201",
              "organizationId": 0,
              "clientId": 1,
              "productId": 6512,
              "useInWildberries": true,
              "useInOzon": true,
              "useInYandexMarket": true,
              "useInAliexpress": false,
              "format": "ean13"
            }
          ],
          "yandexMarketStatus": "UNKNOWN",
          "aliexpressStatus": "UNKNOWN",
          "moySkladStatus": "UNKNOWN",
          "avitoStatus": "UNKNOWN",
          "removeFbsStockOzon": false,
          "removeFbsStockWb": false,
          "removeFbsStockAli": false,
          "removeFbsStockYm": false,
          "removeFbsStockSber": false,
          "price": 630.0000000000001,
          "priceWithoutDiscount": 4200,
          "deliveryCost": 0,
          "wildberriesPrice": 630.0000000000001,
          "wildberriesPriceWithoutDiscount": 4200,
          "salesExpensesOnMpPercent": 20,
          "taxeRate": 0,
          "desiredMarginalityPercent": 0,
          "desiredProfitRub": 0,
          "additionalCost": 0,
          "packWidth": 250,
          "packHeight": 450,
          "totalOrdersCount": 0,
          "totalFbsOrdersCount": 0,
          "emptyBarcodes": false
        }
      },
      "yandexMarketStatus": "UNKNOWN",
      "aliexpressStatus": "UNKNOWN",
      "moySkladStatus": "UNKNOWN",
      "avitoStatus": "UNKNOWN",
      "removeFbsStockOzon": false,
      "removeFbsStockWb": false,
      "removeFbsStockAli": false,
      "removeFbsStockYm": false,
      "removeFbsStockSber": false,
      "price": 630.0000000000001,
      "priceWithoutDiscount": 4200,
      "deliveryCost": 0,
      "wildberriesAverageExpenses": 1,
      "wildberriesPrice": 630.0000000000001,
      "wildberriesPriceWithoutDiscount": 4200,
      "salesExpensesOnMpPercent": 20,
      "taxeRate": 0,
      "desiredMarginalityPercent": 0,
      "desiredProfitRub": 0,
      "additionalCost": 0,
      "packWidth": 250,
      "packHeight": 450,
      "packDepth": 10,
      "totalOrdersCount": 2,
      "totalFbsOrdersCount": 0,
      "wildberriesSaleLogistic": 45,
      "wildberriesReturnLogistic": 50,
      "wildberriesComission": 20,
      "ozonSaleLogistic": 51,
      "ozonReturnLogistic": 51,
      "ozonCommission": 11.5,
      "ozonLastMile": 5.5,
      "emptyBarcodes": false
    }
  ]
}
```

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/find">Полный список полей</a>

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
color | string | Поиск по названию цвета товара
viewId | int64 | Поиск по идентификатору цвета
skuViewId | int64 | Поиск по SKU идентификатору цвета
modelId | int64 | Поиск по идентификатору модели
organizations | int64[] | Ограничивает список организаций
unprofitable | boolean | Товары с отрицательной маржинальностью
needToBuy | boolean | Товары, которые необходимо закупить
hasImages | boolean | Наличие или отсутствие картинок у товара
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
ozonPriceWithoutDiscount | double | Текущая цена без скидки на Ozon
sberMegaMarketPrice | double | Текущая цена на СберМегаМаркет
yandexMarketPrice | double | Текущая цена на Яндекс.Маркет
yandexMarketPriceWithoutDiscount | double | Текущая цена без скидки на Яндекс.Маркет
aliexpressPrice | double | Текущая цена на Aliexpress
aliexpressPriceWithoutDiscount | double | Текущая цена без скидки на Aliexpress
view | ProductView | Объект описывающий Цвет товара
barcodes | array of ProductBarcode | Список штрих-кодов
packWidth | int64 | Ширина упаковки товара в мм
packHeight | int64 | Высота упаковки товара в мм
packDepth | int64 | Глубина/длина упаковки товара в мм
packWeight | int64 | Вес товара в граммах

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
title | string | Название модели для формирования этикетки
name | string | Название модели
brand | Brand | Объект описывающий Бренд товара
category | Category | Объект описывающий Категорию товара
manufacturer | Manufacturer | Объект описывающий Производителя товара

## Информация о карточке

Передавайте параметр params=true, чтобы получить в ответе список значений values.

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/getModelById">Полный список полей</a>

```shell
curl "https://api.selsup.ru/api/product/getModelById?id=1&params=true" \
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

### Тело запроса - ProductModel

Поле | Тип | Описание
--------- | ------- | -----------
id | int64 | Идентификатор модели
title | string | Название модели для формирования этикетки
name | string | Название модели
brand | Brand | Объект описывающий Бренд товара
category | Category | Объект описывающий Категорию товара
manufacturer | Manufacturer | Объект описывающий Производителя товара
views | array of ProductView | Список цветов карточки

### Структура ProductView

Поле | Тип | Описание
--------- | ------- | -----------
id | int64 | Идентификатор цвета
color | string | Название цвета товара
wbArticle | string | Артикул Wildberries
sizes | array of Product | Список размеров карточки

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

## Создание модели

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/create">Полный список полей</a>

```shell
curl -X POST "https://api.selsup.ru/api/product/createModel" \
  -H "Authorization: <token>" -data '{
  "article": "Уникальный артикул товара",
  "organizationId": 123,
  "categoryId": 123,
  "brandId": 123,
  "materials": "content",
  "manufacturerId": 123,
  "packWidth": 10,
  "packHeight": 10,
  "packDepth": 10,
  "packWeight": 1,
  "values": [
    {
      "paramId": 1,
      "stringValue": "text"
    },
    {
      "paramId": 2,
      "doubleValue": 3.0
    }  
  ],
  "views": [
    {
      "color": "Название цвета",
      "mainImageUrl": "http://image.com/1.png",
      "imageUrls": "http://image.com/2.png;http://image.com/3.png",
      "wbArticle": "Артикул на ВБ",
      "values": [
        {
          "paramId": 1,
          "stringValue": "text"
        },
        {
          "paramId": 2,
          "doubleValue": 3.0
        }  
      ],
      "sizes": [
        {
          "name": "Название товара",
          "productType": "PRODUCT",
          "size": "Размер на сайте",
          "realSize": "Российский размер, лучше чтобы совпадал с size",
          "vendorSize": "Размер производителя",
          "barcodes": [
            {
              "barcode": "штрих-код товара"
            }
          ],
          "ozonArticle": "ozon",
          "sberArticle": "sberArticle",
          "leroyMerlinArticle": "leroyMerlinArticle",
          "yandexMarketShopSku": "SKU Яндекса",
          "externalArticle": "externalArticle",
          "packWidth": 10,
          "packHeight": 10,
          "packDepth": 10,
          "packWeight": 1      
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
wildberriesId | int64 | Нет | Номер карточки Wildberries
sizes | array of Product | Да | Список размеров

### Структура Product

Поле | Тип | Обязательность | Описание
--------- | ------- | ------- | -----------
id | int64 | Нет | Идентификатор товара, только при редактировании
size | string | Нет | Размер товара или параметры
name | string | Да | Название товара полное

## Редактирование модели

<a href="https://api.selsup.ru/all.html#tag/Tovary/operation/update">Полный список полей</a>

```shell
curl -X POST "https://api.selsup.ru/api/product/updateModel" \
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

## Работа с параметрами карточки

У каждой структуры ProductModel, ProductView и Product, есть список значений параметров. У каждого параметра есть
идентификатор, который зачастую уникален в рамках всех категорий маркетплейса. Один параметр может повторяться в
разных категориях. Мы всегда пытаемся максимально сохранять идентификатор параметра при любых изменения на
маркетплейсе.

Значения можно определять на разных уровнях, при этом Product имеет самый высший приоритет, потом идут по порядку
значения со следующим приоритетом: ProductView, ProductModel, Category

В некоторых случаях, на маркетплейс не могут быть переданы значения, записанные у размера, например на Wildberries,
тк карточка Wildberries соответствует ProductView. Уровень параметра, указанный в Param.level служит лишь
для определения уровня по умолчанию, на котором должно быть определено значение параметра.

Если у параметра проставлено multiValueAllowed, то может быть несколько ParamValue с одинаковым paramId для
передачи параметров, у которых может быть несколько значений

В зависимости от типа параметра Param.valueType, должны проставляться соответствующие поля в значении ParamValue.

Тип | Параметр для заполнения | Описание
--------- | ------- | -----------
ENUM | option | Должна быть указана структура Option: {\"id\":123,\"name\":"Название"}
TEXT | stringValue | Строковое значение
BOOLEAN | booleanValue | Булевское значение параметра
LONG | longValue | int64 значение
DATE | dateValue | Дата в формате ISO-8601

```shell
curl "https://params.selsup.ru/knowledge/getParams?ozonCategoryId=91025609&wildberriesTypeId=105" \
  -H "Authorization: <token>"
```

> В результате отдается JSON со списком параметров в категории Ozon 91025609 и WB 105

```json
[
  {
    "id": 6920,
    "name": "Бренд в одежде и обуви",
    "published": true,
    "visible": false,
    "serviceParam": "BRAND",
    "useCategoryOption": false,
    "valueType": "ENUM",
    "displayType": "SUGGEST",
    "required": true,
    "multiValueAllowed": false,
    "description": "Укажите наименование бренда, под которым произведен товар. Если товар не имеет бренда, используйте значение \"Нет бренда\"",
    "groupName": "",
    "ozonId": 31,
    "mergeCategoryValues": true,
    "ozonDictionaryId": 28732849,
    "level": "MODEL"
  },
  {
    "id": 6473,
    "name": "Высота упаковки",
    "published": true,
    "visible": false,
    "serviceParam": "PACK_HEIGHT",
    "useCategoryOption": false,
    "valueType": "DOUBLE",
    "displayType": "TEXT",
    "required": false,
    "multiValueAllowed": false,
    "maxValuesCount": 1,
    "wildberriesId": 1,
    "mergeCategoryValues": false,
    "measure": {
      "name": "Высота упаковки",
      "nationalCatalogId": null,
      "wildberriesTypeId": 103,
      "aliexpressCategoryId": null,
      "units": [
        {
          "name": "см"
        }
      ],
      "unitsMap": {
        "см": {
          "name": "см"
        }
      }
    },
    "level": "VIEW"
  }
]
```

Получить список параметров можно через специальный компонент, который ежедневно обновляет список параметров
для каждого маркетплейса. Параметры динамические - они могут постоянно добавляться и удаляться из категории,
когда их правит маркетплейс - тк это параметры маркетплейсов:

https://params.selsup.ru/knowledge/getParams?ozonCategoryId=91025609&wildberriesTypeId=105&avitoCategoryId=103

Можно передать следующие параметры запроса, соответствующие связям категории SelSup с категориями маркетплейсов:

Поле | Тип | Описание
--------- | ------- | -----------
ozonCategoryId | int64 | Идентификатор категории на Ozon
wildberriesTypeId | int64 | Идентификатор категории Wildberries
nationalCatalogId | int64 | ТНВЭД национального каталога
aliexpressCategoryId | int64 | Категория Aliexpress
ymCategoryId | int64 | Категория Яндекс.Маркета
avitoCategoryId | int64 | Категория Авито

### Поля в ответе

Параметр | Тип | Описание
--------- | ------- | -----------
id | int64 | Идентификатор параметра. Используется для сохранения значений ParamValue.paramId
name | string | Название параметра, для avito указывается название тэга
title | string | Название параметра для пользователя, если не указано использовать name
published | boolean | Опубликованность параметра в категории маркетплейса
visible | boolean | Признак того, что параметр скрыт на карточке, тк значение заполняется автоматически
serviceParam | enum | Связь со служебным параметром SelSup из которого берется значение
useCategoryOption | boolean | Передавать в запросе к options.selsup.ru
valueType | enum | Тип значения, которое нужно проставлять в ParamValue
displayType | enum | Вид компонента для отображения параметра в интерфейсе
required | boolean | Признак обязательности параметра
multiValueAllowed | boolean | Признак обязательности параметра
maxValuesCount | int32 | Максимальное количество значений для многозначного параметра
maxLength | int32 | Максимальная длина текстового параметра
maxValue | double | Максимальное значение параметра
minValue | double | Минимальное значение параметра
wildberriesId | int64 | Признак, что параметр Wildberries
service | enum | Сервис, к которому относится параметр
level | enum | Уровень по умолчанию, на котором должен быть определен параметр
measure | Measure | Мера измерения параметра

В некоторых случаях значения параметра, в случае valueType: "ENUM", displayType: "SELECT"
могут отдаваться сразу в Param, но большинство значений параметра для displayType: "SUGGEST",
нужно получать из ответа другого компонента, который отдает значения параметров

## Поиск значений параметра

```shell
curl "https://options.selsup.ru/option/fetchOption?aliexpressCategoryId=201236503&paramId=60019&useCategoryOption=true&query=123&limit=10" \
  -H "Authorization: <token>"
```

> В результате отдается JSON со списком значений параметра 60019 для категории Aliexpress 201236503

```json
[
  {
    "name": "гусиный пух",
    "ozonId": 61811
  },
  {
    "name": "овчина",
    "ozonId": 61978
  }
]
```

Список значений может отдаваться для параметров с Param.valueType: "ENUM" или "TEXT"
В этом случае у них обязательно будет проставлен Param.displayType: "SUGGEST", который говорит о том,
что список значений нужно получать из ответа метода:

https://options.selsup.ru/option/fetchOption?aliexpressCategoryId=201236503&paramId=60019&useCategoryOption=true&query=123&limit=10

Полученные значения необходимо подставлять в качестве option у ParamValue в карточке товара. У значения всегда есть name,
а вот идентификатор может вообще отсутствовать или может соответствовать идентификаторам значений на
маркетплейсах

### Параметры запроса

Параметр | Тип | Описание
--------- | ------- | -----------
limit | int32 | Количество значений, которое отдать
query | string | Поисковый запрос
useCategoryOption | boolean | Необходимо передавать значение соответствующего поля у Param.useCategoryOption
ozonCategoryId | int64 | Идентификатор категории на Ozon
wildberriesTypeId | int64 | Идентификатор категории Wildberries
nationalCatalogId | int64 | ТНВЭД национального каталога
aliexpressCategoryId | int64 | Категория Aliexpress
ymCategoryId | int64 | Категория Яндекс.Маркета
avitoCategoryId | int64 | Категория Авито

## Поиск категорий

<a href="https://api.selsup.ru/all.html#tag/Znaniya-o-tovarah/operation/findCategory">Полный список полей</a>

```shell
curl "https://api.selsup.ru/api/knowledge/findCategory?query=text&limit=50" \
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
curl "https://api.selsup.ru/api/knowledge/findBrand?query=text&limit=50" \
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

<a href="https://api.selsup.ru/all.html#tag/Zakazy/operation/orderNew">Полный список полей</a>

```shell
curl -X POST "https://api.selsup.ru/api/order/" \
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
productId - идентификатор товара в SelSup. Сделать это можно, импортировав все товары методом findProduct

В позиции заказа в товаре quantity обязательно нужно передавать, как и цену товара price.

### Тело запроса Order JSON с ключами

Параметр  | Тип | Обязательный | Описание
--------- | ------- | ------- | -----------
type | enum | Да | Тип заказа, для заказов с сайта необходимо ставить "RETAIL"
products | array of OrderProduct | Да | Список товаров заказа
writeOffStocks | boolean | Нет | Списать остатки товаров под заказ со склада SelSup. Нужно передавать только для заказов с type=FBM. Заказы RETAIL, WHOLESALE по умолчанию всегда резервируют остатки 
stockWarehouseId | int64 | Нет | Идентификатор вашего склада в SelSup, с которого списать остатки товаров. Отдается в методе getApplicationData. Не обязательно передавать, если у вас 1 склад
organizationId | int64 | Нет | Идентификатор организации. Необязательный если в аккаунте 1 организация. Отдается в методе getApplicationData.
comment | string | Нет | Текстовый комментарий к заказу
name | string | Нет | Название заказа для быстрого поиска в списке

### Структура OrderProduct

Параметр  | Тип | Обязательный | Описание
--------- | ------- | ------- | -----------
id | int64 | Нет | Идентификатор позиции в заказе, передается при изменении позиции заказа
quantity | int32 | Да | Количество товара в заказе
price | double | Нет | Цена товара, для type=FBM не обязательна
otherExpenses | double | Нет | Дополнительные расходы на товар
productId | int64 | Да | Идентификатор товара в SelSup. Передается в ответе findProduct

## Создание отгрузки на маркетплейс

```shell
curl -X POST "https://api.selsup.ru/api/order/" \
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

`POST https://api.selsup.ru/api/order`

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
curl "https://api.selsup.ru/api/order/findOrder?type=FBS&actual=true&count=false&limit=100&page=1" \
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

Параметр count отвечает за подсчет общего количества заказов по указанному запросу. Лучше передавать значение false,
если вам не нужно знать общее количество заказов, тк подсчет количества может занимать продолжительное время, 
особенно если по запросу выбирается большое количество заказов. Лучше запрашивать постоянно изменяя параметр
page, чтобы выбрать все данные, пока количество равно лимиту, который вы передаете в запросе.

## Получение изменений заказов

Вы можете выбирать заказы, которые изменились с последней даты получения заказов. При этом мог изменится 
состав заказа, параметры заказа или статус. В заказе отдается поле modifiedDate по которому вы можете выбирать
заказы. В фильтрах есть поле modifiedDate в котором указывается дата и отдаются заказы которые изменились
начиная с указанной даты

```shell
curl "https://api.selsup.ru/api/order/find?type=FBS&modifiedDate=2024-06-20T15:00:00Z" \
  -H "Authorization: <token>"
```

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

### Структура Order

Поле | Тип | Описание
--------- | ------- | -----------
id | int64 | Уникальный идентификатор заказа
name | string | Название заказа, в основном проставляется для отгрузки на маркетплейс
type | enum | Тип заказа, возможные значения: "FBM" - Отгрузка на склад маркетплейса, "FBS" - Отгрузка со своего склада, "RETAIL" - Розничный заказ, "WHOLESALE" - Оптовая продажа, "FBO" - Отгрузка заказа со склада маркетплейса
boxType | enum | Тип коробов, для отгрузки на маркетплейс
service | enum | Маркетплейс или сервис, с которого пришел заказ
writeOffStocks | boolean | Списать остатки товаров со склада SelSup
stockWarehouseId | int64 | Идентификатор склада SelSup, с которого списать остатки
warehouseId | int64 | Идентификатор склада маркетплейса, на который отгружается заказ
deliveryDate | string | Дата доставки заказа в формате ISO-8601, например 2019-08-24T14:15:22Z
invoiceNumber | int64 | Номер накладной - формируется автоматически по порядку с начала года
externalOrderId | string | Внешний номер заказа, используется для проверки уникальности при указании service и type
organizationId | int64 | Идентификатор организации в SelSup
products | array of OrderProduct | Список позиций заказа
supply | Supply | Поставка, объединяющая несколько заказов
comment | string | Комментарий к заказу
serviceWarehouseId | string | Склад маркетплейса с которого пришел заказ
logistics | int64 | Расходы на логистику по заказу
saleDate | string | Дата продажи заказа в формате ISO-8601, например 2019-08-24T14:15:22Z
returnDate | string | Дата возврата заказа покупателем в формате ISO-8601, например 2019-08-24T14:15:22Z
deliveryService | enum | Служба доставки для заказов DBS или розничных/оптовых
deliveryServiceIdentification | string | Номер заказа в службе доставки
deliveryPoint | string | JSON с информацией о точке доставки заказа для выбранной службы доставки
deliveryPointCode | string | Идентификатор точки доставки заказа для выбранной службы доставки
deliveryLabelPath | string | Путь в хранилище до файла с этикеткой доставки заказа
labelPath | string | Путь в хранилище до файла с этикеткой заказа
stickerId | int64 | Числовой штрих-код на этикетки заказа
stickerText | string | Текстовый штрих-код на этикетки заказа

# Остатки

## Остатки в SelSup

Остатки товаров в SelSup привязываются к SKU - единице хранения на складе. Каждому товару присваивается свой номер SKU и в дальнейшем можно указать одинаковый SKU для нескольких разных товаров в SelSup.

Вы можете использовать две схемы хранения остатков в SelSup:
1)Когда на каждую штуку товара клеится отдельный уникальный код, по которому можно отслеживать всю историю товара и вы всегда можете отделить каждую единицу товара друг от друга. Данный уникальный стикер позволяет вам клеить его в удобное для быстрого поиска место товара, что существенно ускоряет сборку товаров на складе и их идентификацию - особенно если вы работаете с кодами маркировки честного знака
2)Когда остаток хранится просто к привязки к ячейке по штрих-коду. В этом случае в остатке записывается количество - сколько лежит определенного товара в данной ячейке.

## Изменение остатков

<a href="https://api.selsup.ru/all.html#tag/Hranenie-tovara-na-sklade-(WMS)/operation/changeStock">Полный список полей</a>

```shell
curl -X POST "https://api.selsup.ru/api/wms/changeStock?skuId=123&stock=5&warehouseId=123" \
  -H "Authorization: <token>"
```

> В результате отдается 200 код ответа или 400 в случае ошибки

Позволяет для SKU изменить остатки товаров на складе. Работает для всех схем хранения, как с уникальными кодами, так и без них

# Добавление функций в магазин приложений

Вы можете разрабатывать расширения SelSup, которые добавляют различные возможности в SelSup. Существует несколько
возможных вариантов встраивания функций в SelSup

## React-расширения

Вы можете разрабатывать расширения для SelSup реализуя функциональные React компоненты, которые встраиваются 
в различные места кабинета SelSup и взаимодействуют с API SelSup или API внешних сервисов. Внешнему сервису 
необходимо разрешить принимать запросы с домена selsup.ru. При этом вы можете использовать все стандартные 
компоненты SelSup и добавлять свои собственные

Клонируйте репозиторий демо-компонента SelSup и начните разрабатывать React-расширение.

<a href="https://github.com/SelSup/component">https://github.com/SelSup/component</a>

## Backend интеграции

Вы можете реализовать на Java один из вариантов интеграции: маркетплейс (интеграция по остаткам, заказам, товарам, 
ценам) или служба доставки, реализовав соответствующий интерфейс SelSup. Код компонента попадет в основную
ветку SelSup и будет доступен для использования вашим платным или бесплатным расширением. Вы сможете обновлять функции 
вашего расширения и изменения будут регулярно попадать в новые релизы SelSup.
