# Service Name: Customer
## customer.profile.upsert
|Description    |insert/update customer information|
|--|--|
|Partition      |  |
|Data Type      | json |
|EventKey       | INSERT/UPDATE |
|Example `Payload`|{ "cif": string, "sid": string, "customer_name": string, "risk_profile": string, "risk_profile_expiration_date": date, "status", string, "tenant_code": "string" }| `
|Producer       | Customer Service |
|Consumer(s)       | Portfolio Service |
|Notes          |  |


# Service Name: Portfolio
## portfolio.pfolio.entry (deprecated, event moved to `customer.profile.upsert`)

|Description    | This topic handles the creation of a customer portfolio record and inserts the data into the `portfolio_customers` table. The incoming payload is processed and converted to a `PortfolioCustomer` domain model before being saved to the database.|
|--|--|
|Partition      |  |
|Data Type      | json |
|Example `Payload`|{"sid": "encryptedSID123","cif": "CIF123456","tenant_code": "TENANT001","is_default": true}| `
|Producer       |  |
|Consumer(s)       | Portfolio Service |
|Notes          | The message payload is of type `CustomerPortfolioDefault`, which is transformed into a `PortfolioCustomer` model. The `PortfolioCustomer` is then inserted into the `portfolio_customers` database table. |

## portfolio.pfolioaccount.entry

|Description    |insert new record of portfolio products|
|--|--|
|Partition      |  |
|Data Type      | json |
|Example `Payload`|{ "order_id": int, "portfolio_id": int, "portfolio_name": "string", "portfolio_product_id": int, "tenant_code": "string" }| `
|Producer       | Portfolio Service  |
|Consumer(s)       | Order Service |
|Notes          |  |

# Service Name: Order
## oms.order.created

|Description    | Topic ini menangani pembuatan order dan memasukan data ke dalam table `orders`. Payload yang masuk diproses dan di konversi ke model `orderevent` setelah disimpan ke database |
|--|--|
|Partition      |  |"
|Header         | CorrelationID: `87f78805-9228-4b50-9038-6ba4326b3b28`, TenantCode: `praisindo_qpgk4g` |
|EventKey | `ENTRIED`, `CORRECTED` |
|Data Type      | json |
|Example `Payload`| {"order_id":105,"product_id":2,"customer_id":1507,"order_type_id":"1","order_type_name":"Buy","order_plan_id":null,"order_plan_name":null,"portfolio_account_no":null,"portfolio_account_type":null,"portfolio_account_id":null,"cif":"730048947","sid":"2519501192345","asset_class_id":"FI","sub_asset_class_id":"CB","product_code":"BND002","product_name":"Corporate Bond High Yield","investment_amount":null,"settlement_amount":null,"tenor":null,"face_value":20000} |
|Producer       | order service |
|Consumer       |  |
|Notes          |  |

## oms.order.change_status

|Description    | Topic ini menangani perubahan status order `orders`. Payload yang masuk diproses dan di konversi ke model `orderevent` setelah melakukan update ke database. |
|--|--|
|Header | CorrelationID: `87f78805-9228-4b50-9038-6ba4326b3b28`, TenantCode: `praisindo_qpgk4g` |
|Partition      |  |
|Data Type      | json |
|EventKey | VERIFIED, UNVERIFIED, PENDSETTLE, SETTLED, SENDBACK, REJECTED, DELETED |
|Example `Payload`| {"order_id":105,"product_id":2,"customer_id":1507,"order_type_id":"1","order_type_name":"Buy","order_plan_id":null,"order_plan_name":null,"portfolio_account_no":null,"portfolio_account_type":null,"portfolio_account_id":null,"cif":"730048947","sid":"2519501192345","asset_class_id":"FI","sub_asset_class_id":"CB","product_code":"BND002","product_name":"Corporate Bond High Yield","investment_amount":null,"settlement_amount":null,"tenor":null,"face_value":20000} |
|Producer       | order service |
|Consumer       |  |
|Notes          |  |
