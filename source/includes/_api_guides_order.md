## Get list orders

The API used when you want to get the order list from TIKI.

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">GET</i>
		<h6>https://api.tiki.vn/integration/v1/orders</h6>
	</div>
</div>

> Example query:

```http
GET https://api.tiki.vn/integration/v1/orders
```

> List order response body

```json
{
    "data": [
        {
            "order_code": "929231617",
            "coupon_code": "ABC",
            "status": "queueing",
            "total_price_before_discount": 200000,
            "total_price_after_discount": 205000,
            "updated_at": "2019-10-30 17:27:24",
            "purchased_at": "2019-10-30 17:27:17",
            "fulfillment_type": "seller_delivery",
            "note": "",
            "is_rma": null,
            "discount": {
                "discount_amount": 10000,
                "discount_coupon": 1000
            },
            "tax": {
                "code": "123",
                "name": "nguyen van A",
                "address": "Ha Noi, Vietnam"
            },
            "shipping": {
                "name": "hậu nguyễn",
                "street": "519 kim mã",
                "ward": "Phường Kim Mã",
                "city": "Quận Ba Đình",
                "region": "Hà Nội",
                "country": "VN",
                "phone": "0912611089",
                "estimate_description": "Dự kiến giao hàng vào Thứ hai, 04/11/2019",
                "shipping_fee": 15000
            },
            "items": [
                {
                    "id": 25203463,
                    "product_id": 2050232,
                    "product_name": "Cơm gà - dropship -hn4",
                    "sku": "9956112228645",
                    "original_sku": "123",
                    "qty": 1,
                    "price": 200000,
                    "confirmation_status": "seller_confirmed",
                    "confirmed_at": "2019-11-01 10:07:58",
                    "must_confirmed_before_at": "2019-11-01 23:59:59",
                    "inventory_type": "seller_backorder"
                }
            ],
            "payment": {
                "payment_method": "cod",
                "updated_at": "2019-10-30 17:27:25",
                "description": "Thanh toán tiền mặt khi nhận hàng"
            },
            "handling_fee": 10000,
            "collectable_total_price": 205000
        }
    ],
    "paging": {
        "total": 197,
        "per_page": 1,
        "current_page": 1,
        "last_page": 197,
        "from": 1,
        "to": 1
    }
}
```

We only support [get list orders](#api-get-list-orders) of the last 30 days.

By default, you will get orders in the queueing state, queueing is state order waiting for seller confirm

In addition to status queueing, we support you to query according to some of the status below:

| Status                      | Description                                | User for |
| --------------------------- | ------------------------------------------ | -------- |
| seller_confirmed            | Seller has confirmed this order before     | Get list order confirmed |
| seller_canceled             | Seller has canceled this order before      | Get list order canceled |
| complete                    | The order has been delivered successfully  | Get list order complete |

In the informative [order](#order), we describe some important fields that you need to pay attention to:

#### tax

|Field                        | Description                                |
| --------------------------- | ------------------------------------------ |
| tax.code                    | tax code of customer                       |
| tax.name                    | tax name of customer                       |
| tax.address                 | tax address of customer                    |

Example:

* _tax.code_ = "123": tax code of customer is "123"
* _tax.name_ = "nguyen Van A": tax name of customer is "nguyen van A"
* _tax.address_ = "Ha Noi, Vietnam"": tax address of customer is "Ha Noi, Vietnam""


#### payment

|Field                       | Description                                |
| --------------------------- | ------------------------------------------ |
| payment.payment_method      | Payment method                             |

Example:

* _payment.payment_method_ = "cod": the payment method is cod, shipper needs to collect money from the user
* _payment.payment_method_ = "VISA": the payment method is VISA, shipper doesn't needs to collect money from the user


#### shipping
info of customer such as address, email, phone,
Based on TIKI's commitment to confidentiality with customers, we can only publish personal information such as email and phone numbers if you register as seller delivery

|Field                        | Description                                |
| --------------------------- | ------------------------------------------ |
| shipping.street             | the street of customer for delivery |
| shipping.shipping_fee       | the fee for shipping       |
| shipping.phone              | the phone of customer, only display for seller_delivery      |
| shipping.email              | the email of customer, only display for seller_delivery      |


Example: 

* _shipping.street_ = "519 kim mã": the street of customer for delivery is "519 kim mã"
* _shipping.phone_ = "0912611089": the phone number of customer is "0912611089"
* _shipping.shipping_fee_ = 15000: the fee for shipping is 15000 VND


#### fulfillment_type

|Field                       | Description                                |
| --------------------------- | ------------------------------------------ |
| fulfillment_type            | Order fulfillment                          |

Example:

* **fulfillment_type**: _fulfillment types_ is mode of operation of the order specified by TIKI.
    * **tiki_delivery**: This order will be delivery by TIKI
    * **seller_delivery**: This order will be delivery by Seller (it you)
    * The order may be TIKI delivery or seller delivery depending on the type of product or operation you have registered with tiki


#### price/fee
|Field                       | Description                                |
| --------------------------- | ------------------------------------------ |
| shipping_fee                | the fee for shipping       |
| handling_fee                 | the cost of order need collect if orders bulky       |
| collectable_total_price     | total amount the shipper needs to collect from the customer      |
| total_price_before_discount | Total order amount before discounts        |
| total_price_after_discount  | Total order amount after applied discounts |
| discount.discount_amount  | Total amount discount on the order |

We have the recipe: _total_price_after_discount = total_price_before_discount + shipping_fee + handling_fee - discount_amount_


Example:

* _total_price_after_discount_ = 205000: The total amount of money the user needs to pay for the order is 205000 VND
* _coupon_code_ = ABC: The coupon code that the user has used is "_ABC_" with value is _discount.discount_coupon_ = 1000 VND
* _discount.discount_amount_ = 10000: Total amount discount on the order is 10000 VND
* _handling_fee_ = 10000: the cost of order need collect if orders bulky
* _collectable_total_price_ = 205000: total amount shipper to be collected from customer

#### items

|Field                       | Description                                |
| --------------------------- | ------------------------------------------ |
| item.original_sku           | the seller product code of item            |
| item.must_confirmed_before_at | Orders item need to be confirmed before this time |

Example:

* _item.original_sku_ = "123": seller product code/sku is "123"
* _item.must_confirmed_before_at_ = "2019-11-01 23:59:59": Orders item need to be confirmed before "2019-11-01 23:59:59"


## Confirm an order

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">POST</i>
		<h6>https://api.tiki.vn/integration/v1/orders/confirmItems</h6>
	</div>
</div>

> Example confirm an order
 
```shell
curl --location --request POST 'https://api-sandbox.tiki.vn/integration/v1/orders/confirmItems' \
--header 'Content-Type: application/json' \
--header 'tiki-api: 55f438d1-3438-409e-b5a4-9d16e764c5b8' \
--data-raw '{
  "order_code": "929231617",
  "warehouse_id": 903,
  "item_ids": [25205113],
  "delivery_commitment_time": "2019-12-03 23:59:59",
  "tracking_number": "929231617"
}'
```

If your order is **tiki_delivery** congratulation, you don't need to confirm the order as we have supported automatic order confirmation with this type.

Before confirm order you need to determine which products are in stock.

_**Warehouse**_ contains information about the address, contact point about the location of products that you have registered with TIKI.

We support API [get warehouse](#api-get-warehouses) to see the list warehouse you registered before. If you don't see any match warehouse you can tell _seller support_ to add it manually.

> Warehouse response body

```json
[
    {
        "contact_email": "fhds@gmail.com",
        "contact_name": "lan",
        "contact_phone": "0989777666",
        "country": {
            "code": "vn",
            "name": "Viet Nam"
        },
        "district": {
            "code": "VN039007",
            "name": "Quận 10"
        },
        "name": "kho 3-hcm",
        "region": {
            "code": "VN039",
            "name": "Hồ Chí Minh"
        },
        "street": "cmt8",
        "ward": {
            "code": "VN039007001",
            "name": "Phường 01"
        },
        "warehouse_id": 900
    }
]
```        

From this warehouses response you need to note:

* _**warehouse_id**_: The id identifies your warehouse in the Tiki system


After customer place an order, seller have to send a [confirm order items](#api-confirm-order-items) to make sure your product is still available. 
This is a important step before delivery product to customer so please confirm it as soon as possible 

Remember the two fields that I told you to save to confirm your order. 
Please provide **_warehouse_id_** with _**order_code**_ & your list of available order item id.
You need these select item for confirm and add item to **item_ids**

**Example1**: your order has 5 items as _[1, 2, 3, 4, 5]_ but only 2 items _[1, 3]_ of them are in stock. Just add these 2 item id to the _**item_ids**_, we will implicitly assume that the other 3 _items=[2, 4, 5]_ are no longer available. Now the _item_ids=[1, 3]_


**Example2** if all of your items is not available then give us an empty _**item_ids**_ list. Now _item_ids=[]_


In addition to the **warehouse_id** and **item_ids** parameters, you need to pay extra attention:

* _delivery_commitment_time_: It is the time that you commit to deliver to customers, String datetime with format Y-m-d H:i:s.
* _tracking_number_: maybe equal order code, tracking_number is code for tracking order via 3rd party system or anything like this

_**Note**_: Don't worry, TIKI has supported all products in one order, then it will be in stock.

## Update delivery status
 
<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">POST</i>
		<h6>https://api.tiki.vn/integration/v1/orders/updateDeliveryStatus</h6>
	</div>
</div>

```shell
curl --location --request POST 'https://api-sandbox.tiki.vn/integration/v1/orders/updateDeliveryStatus' \
--header 'Content-Type: application/json' \
--header 'tiki-api: 55f438d1-3438-409e-b5a4-9d16e764c5b8' \
--data-raw '{
    "order_code": "401734337",
    "update_time":"2020-11-23 23:59:59",
    "status": "successful_delivery"
}'
```

If your order is **tiki_delivery** congratulation, TIKI will help you complete this order.

When you are seller delivery you need to update the status of the order, so that tiki updates to the system and sends information to customers.

You have to [update delivery status](#api-update-delivery-status) step by step whenever you reach a new status in this list 

You need map from your delivery status to _**tiki delivery status**_:

* _transferring_to_foreign_warehouses_: Orders transferring to your foreign warehouses
* _has_come_to_foreign_warehouses_: Orders has come to your foreign warehouses
* _rotating_to_vietnam_: Orders rotating to vietnam
* _customs_clearance_: Customs clearance processing
* _customs_clearance_complete_: Customs clearance complete
* _item_arrived_in_vietnam_: Orders arrived in vietnam
* _ready_for_delivery_: Orders ready for delivery
* _on_delivery_: Orders on delivery
* _successful_delivery_: Orders successful delivery

Finally, your order delivery status becomes **successful delivery**, everything is settled.

## Print order label

<div class="api-endpoint">
	<div class="endpoint-data">
		<i class="label label-get">GET</i>
		<h6>https://api.tiki.vn/integration/v1/orders/905205190/print</h6>
	</div>
</div>

[This API](#print-order-labels) is used when you are **seller delivery**, it is used to print order label for shipping.

```shell
curl --location --request GET 'https://api-sandbox.tiki.vn/integration/v1/orders/905205190/print' \
--header 'tiki-api: 55f438d1-3438-409e-b5a4-9d16e764c5b8'
```

> Response example:

```json
{
    "shipping_label_url": "https://uat.tikicdn.com/ts/print/0d/71/f7/c68cf4ba4b29d1f79bac442a2ec2aa42.html"
}
```

Example order label:
![](https://salt.tikicdn.com/ts/files/cf/51/bf/edb4c126a9124dbf76e6d519d4e06310.png)

In this example orders label you need note:

* **1)**: orders code
* **2)**: the mode of payment that the user has used
* **3)**: the address, phone of customer for delivery
* **4)**: total amount shipper to be collected from customer