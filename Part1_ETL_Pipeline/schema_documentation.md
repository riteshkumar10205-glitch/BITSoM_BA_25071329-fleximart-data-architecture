1. Entity-Relationship Description:

ENTITY: customers
Purpose: Stores customer information
Attributes:
- customer_id: Unique identifier (Primary Key)
- first_name: Customer's first name
- last_name: Customer's family name.
- email: Unique contact address for login and notifications.
- phone: Contact number for delivery updates.
- city: Geographic location for regional sales analysis.
- registration_date: The date the customer joined the platform.

ENTITY: products
Purpose: Maintains the master catalog of items available for sale.
Attributes:
- product_id (PK): Unique identifier for each product.
- product_name: Title of the product.
- category: Broad classification (e.g., Electronics).
- price: Current price per single unit.
- stock_quantity: Current inventory levels.

ENTITY: orders
Purpose: Tracks high-level transaction headers.
Attributes:
- order_id (PK): Unique transaction number.
- customer_id (FK): Links the order to a specific customer.
- order_date: Timestamp of the transaction.
- total_amount: The sum total of all items in the order.
- status: Current state of the order (e.g., Pending, Shipped, Cancelled).

ENTITY: order_items
Purpose: A junction/line-item table capturing specific products within an order.
Attributes:
- order_item_id (PK): Unique ID for each row.
- order_id (FK): Links to the parent order.
- product_id (FK): Links to the specific product purchased.
- quantity: Number of units of this product.
- unit_price: Price per unit at the time of sale (historical snapshot)
- subtotal: Calculated value (quantity x unit_price).


Relationships:
customers to orders (1:M): One customer can place MANY orders over time, but each order belongs to only one customer.

orders to order_items (1:M): One order can contain MANY products (items), but each line item belongs to exactly one order.

products to order_items (1:M): One product can appear in MANY different orders, but each line item refers to a single product.


2. Normalization Explanation:
A. This database design follows Third Normal Form (3NF). This ensures data integrity and minimizes redundancy. 
a) 1NF is satisfied as all attributes are atomic, without any repeating groups, e.g. first_name and last_name.
b) 2NF is achieved because all the non-key attributes are fully functionally dependent on the primary, e.g. in the order_items table, quantity, unit_price and subtotal depend entirely on order_item_id and not partially on order_id or product_id alone. 
c) 3NF is satisfied because of the lack of transitive dependencies. For e.g., customer city is stored only in the customers table and not repeated in the orders table, even though orders reference customers. 

B. In this schema, the functional dependencies are as follows:
a) customer_id - first_name, last_name, email, phone, city, registration_date. All personal details are uniquely determined by the customer_id.
b) product_id - product_name, category, price, stock_quantity. The attributes of a product are strictly tied to the product_id
c) order_id - customer_id, order_date, total_amount, status. The order_id uniquely identifies who bought it and when. 
d) order_item_id - order_id, product_id, quantity, unit_price, subtotal. Each order_item_id uniquely determines a single order line item.

C. The 3NF structure avoids 3 types of anomalies:
a) Insert Anomaly- A new product can be added to the products table withough needing an existing order.
b) Update Anomaly- In case of an updation of say a customer's email id, we can update it in one row in the customers table.
c) Delete Anomaly- If we delete a cancelled order from the orders table, the order_items are removed without losing the customer's existing orders or information. 


3. Sample Data Representation

a. Customers Table:
| customer_id | first_name | last_name | email | phone | city | registration_date | customer_sk |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | ---: |
| C001 | Rahul | Sharma | rahul.sharma@gmail.com | +919876543210 | Bangalore | 2023-01-15 | 1 |
| C002 | Priya | Patel | priya.patel@yahoo.com | +919988776655 | Mumbai | 2023-02-20 | 2 |
| C003 | Amit | Kumar | unknown@example.com | +919765432109 | Delhi | 2023-03-10 | 3 |

b. Products Table:
| product_id | product_name | category | price | stock_quantity | product_sk |
| :--- | :--- | :--- | ---: | ---: | ---: |
| P001 | Samsung Galaxy S21 | Electronics | 45999 | 150 | 1 |
| P002 | Nike Running Shoes | Fashion | 3499 | 80 | 2 |
| P003 | Apple MacBook Pro | Electronics | 2999 | 45 | 3 |

c. Orders Table:
| order_id | customer_id | product_id | quantity | unit_price | order_date | status | total_amount | order_sk |
| :--- | :--- | :--- | ---: | ---: | :--- | :--- | ---: | ---: |
| T001 | C001 | P001 | 1 | 45999 | 2024-01-15 | Completed | 45999 | 1 |
| T002 | C002 | P004 | 2 | 2999 | 2024-01-16 | Completed | 5998 | 2 |
| T005 | C005 | P009 | 3 | 650 | 2024-01-20 | Completed | 1950 | 3 |

d. Order Items Table:
| order_id | product_id | quantity | unit_price | subtotal |
| :--- | :--- | ---: | ---: | ---: |
| T001 | P001 | 1 | 45999 | 45999 |
| T002 | P004 | 2 | 2999 | 5998 |
| T005 | P009 | 3 | 650 | 1950 |