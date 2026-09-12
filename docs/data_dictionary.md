# Data Dictionary

Описание структуры исходных данных Olist Brazilian E-Commerce Public Dataset.

## Table Overview

| Table | Grain | Primary Key | Description |
|---|---|---|---|
| `data_customers` | 1 row = 1 customer record | `customer_id` | Информация о клиентах |
| `data_geolocation` | 1 row = 1 geolocation record | — | Географические координаты почтовых индексов |
| `data_order_items` | 1 row = 1 item within an order | `order_id` + `order_item_id` | Состав заказов |
| `data_order_payments` | 1 row = 1 payment associated with an order | `order_id` + `payment_sequential` | Информация об оплатах заказов |
| `data_order_reviews` | 1 row = 1 review | `review_id` | Отзывы клиентов о заказах |
| `data_orders` | 1 row = 1 order | `order_id` | Информация о заказах |
| `data_products` | 1 row = 1 product | `product_id` | Информация о товарах |
| `data_sellers` | 1 row = 1 seller | `seller_id` | Информация о продавцах |
| `data_product_category_name_tr` | 1 row = 1 product category | `product_category_name` | Перевод названий категорий товаров |

---

# `data_customers`

Информация о клиентах, совершавших заказы.

**Grain:** 1 строка = 1 запись о клиенте.

| Column | Key | Description |
|---|---|---|
| `customer_id` | PK | Уникальный идентификатор записи клиента. Используется для связи с таблицей `data_orders`. |
| `customer_unique_id` | — | Уникальный идентификатор клиента, позволяющий идентифицировать одного клиента между различными заказами. |
| `customer_zip_code_prefix` | FK | Префикс почтового индекса клиента. Может использоваться для связи с географическими данными. |
| `customer_city` | — | Город клиента. |
| `customer_state` | — | Штат/регион клиента. |

---

# `data_geolocation`

Географические данные, связанные с почтовыми индексами.

**Grain:** 1 строка = 1 запись о географической точке/локации.

| Column | Key | Description |
|---|---|---|
| `geolocation_zip_code_prefix` | — | Префикс почтового индекса, связанный с географической локацией. |
| `geolocation_lat` | — | Географическая широта. |
| `geolocation_lng` | — | Географическая долгота. |
| `geolocation_city` | — | Город, соответствующий географической записи. |
| `geolocation_state` | — | Штат/регион, соответствующий географической записи. |

> **Note:** `geolocation_zip_code_prefix` не следует автоматически считать уникальным ключом. Один почтовый индекс может соответствовать нескольким географическим точкам.

---

# `data_order_items`

Состав заказов. Каждая запись соответствует отдельной товарной позиции внутри заказа.

**Grain:** 1 строка = 1 товарная позиция в заказе.

| Column | Key | Description |
|---|---|---|
| `order_id` | FK | Уникальный идентификатор заказа. |
| `order_item_id` | — | Порядковый номер товарной позиции внутри заказа. |
| `product_id` | FK | Идентификатор товара. |
| `seller_id` | FK | Идентификатор продавца. |
| `shipping_limit_date` | — | Крайний срок передачи товара продавцом для отправки. |
| `price` | — | Цена товарной позиции. |
| `freight_value` | — | Стоимость доставки товарной позиции. |

**Composite key:** `order_id` + `order_item_id`.

---

# `data_order_payments`

Информация о платежах, связанных с заказами.

**Grain:** 1 строка = 1 платёжная запись для заказа.

| Column | Key | Description |
|---|---|---|
| `order_id` | FK | Уникальный идентификатор заказа. |
| `payment_sequential` | — | Порядковый номер платежа внутри заказа. |
| `payment_type` | — | Тип способа оплаты. |
| `payment_installments` | — | Количество платежей/рассрочки. |
| `payment_value` | — | Сумма платежа. |

**Composite key:** `order_id` + `payment_sequential`.

---

# `data_order_reviews`

Отзывы клиентов о заказах.

**Grain:** 1 строка = 1 отзыв.

| Column | Key | Description |
|---|---|---|
| `review_id` | PK | Уникальный идентификатор отзыва. |
| `order_id` | FK | Идентификатор заказа, к которому относится отзыв. |
| `review_score` | — | Оценка заказа клиентом. |
| `review_comment_title` | — | Заголовок текстового отзыва. |
| `review_comment_message` | — | Текст отзыва. |
| `review_creation_date` | — | Дата создания отзыва. |
| `review_answer_timestamp` | — | Дата и время ответа на отзыв. |

---

# `data_orders`

Основная информация о заказах.

**Grain:** 1 строка = 1 заказ.

| Column | Key | Description |
|---|---|---|
| `order_id` | PK | Уникальный идентификатор заказа. |
| `customer_id` | FK | Идентификатор клиента, создавшего заказ. |
| `order_status` | — | Текущий статус заказа. |
| `order_purchase_timestamp` | — | Дата и время создания заказа. |
| `order_approved_at` | — | Дата и время подтверждения оплаты заказа. |
| `order_delivered_carrier_date` | — | Дата передачи заказа службе доставки. |
| `order_delivered_customer_date` | — | Дата доставки заказа клиенту. |
| `order_estimated_delivery_date` | — | Расчётная дата доставки заказа. |

---

# `data_products`

Информация о товарах.

**Grain:** 1 строка = 1 товар.

| Column | Key | Description |
|---|---|---|
| `product_id` | PK | Уникальный идентификатор товара. |
| `product_category_name` | FK | Название категории товара на исходном языке. Может использоваться для связи с `data_product_category_name_tr`. |
| `product_name_lenght` | — | Длина названия товара. |
| `product_description_lenght` | — | Длина описания товара. |
| `product_photos_qty` | — | Количество фотографий товара. |
| `product_weight_g` | — | Вес товара в граммах. |
| `product_length_cm` | — | Длина товара в сантиметрах. |
| `product_height_cm` | — | Высота товара в сантиметрах. |
| `product_width_cm` | — | Ширина товара в сантиметрах. |

> **Note:** `product_name_lenght` и `product_description_lenght` содержат опечатку `lenght` в названии исходного датасета. При проектировании собственной схемы БД название можно исправить на `length`, но в документации исходных данных лучше сохранить оригинальное название.

---

# `data_sellers`

Информация о продавцах.

**Grain:** 1 строка = 1 продавец.

| Column | Key | Description |
|---|---|---|
| `seller_id` | PK | Уникальный идентификатор продавца. |
| `seller_zip_code_prefix` | FK | Префикс почтового индекса продавца. |
| `seller_city` | — | Город продавца. |
| `seller_state` | — | Штат/регион продавца. |

---

# `data_product_category_name_tr`

Справочник соответствия названий категорий товаров на исходном языке и английском языке.

**Grain:** 1 строка = 1 категория товара.

| Column | Key | Description |
|---|---|---|
| `product_category_name` | PK | Название категории товара на исходном языке. |
| `product_category_name_english` | — | Название категории товара на английском языке. |

---

# Key Relationships

Основные связи между таблицами:

| Parent table | Child table | Key | Relationship |
|---|---|---|---|
| `data_customers` | `data_orders` | `customer_id` | 1:N |
| `data_orders` | `data_order_items` | `order_id` | 1:N |
| `data_products` | `data_order_items` | `product_id` | 1:N |
| `data_sellers` | `data_order_items` | `seller_id` | 1:N |
| `data_orders` | `data_order_payments` | `order_id` | 1:N |
| `data_orders` | `data_order_reviews` | `order_id` | 1:N |
| `data_products` | `data_product_category_name_tr` | `product_category_name` | N:1 |
| `data_customers` | `data_geolocation` | `customer_zip_code_prefix` → `geolocation_zip_code_prefix` | N:1* |
| `data_sellers` | `data_geolocation` | `seller_zip_code_prefix` → `geolocation_zip_code_prefix` | N:1* |

\* Связь с `data_geolocation` требует отдельного внимания, поскольку один `geolocation_zip_code_prefix` может иметь несколько географических записей.

---

# Notes

## Customer identifiers

В данных присутствуют два идентификатора клиента:

- `customer_id` — идентификатор записи клиента, используемый для связи с заказом;
- `customer_unique_id` — идентификатор самого клиента, позволяющий определить повторные заказы одного и того же клиента.

При расчёте клиентских метрик необходимо определить, какой из идентификаторов является аналитически корректным.

## Geolocation

Таблица `data_geolocation` имеет особую структуру: один `geolocation_zip_code_prefix` может встречаться несколько раз и соответствовать нескольким координатам.

Поэтому перед использованием таблицы для JOIN необходимо определить правило выбора или агрегации географической точки.

## Original column names

Названия колонок в данном документе соответствуют исходному датасету Olist. Возможные исправления названий и преобразования типов следует фиксировать отдельно на этапе проектирования целевой БД.