# WitleShop Online Retail System - Database Design

**Author:** Ntwanano Phiona Shabane

## 📌 Overview
This repository contains the complete database design documentation for **WitleShop (Pty) Ltd**, a fast-growing South African online retailer selling electronics, clothing, and home appliances.

The design translates the business requirements into a logical data model, identifying all entities, attributes, primary/foreign keys, relationships, and cardinalities. It includes a full Entity Relationship Diagram (ERD) using Crow's Foot notation and is normalised to Third Normal Form (3NF).

## 📂 Repository Contents
*   **`Ntwanano Phiona Shabane - WitleShop Database Design.pdf`** – The full database design documentation, including the data dictionary, ERD, design decisions, and normalisation check.
*   **`schema.sql`** – PostgreSQL script that creates all nine tables with their keys, constraints, a delivery-address trigger and indexes.

## 🗄️ Database Schema
The database consists of **nine entities** designed to manage the core business areas:

| Entity | Description | Type |
|---|---|---|
| **CUSTOMER** | Registered buyer with unique email and multiple addresses. | Strong Entity |
| **ADDRESS** | Delivery addresses belonging to a customer. | Dependent Entity |
| **CATEGORY** | Product groupings (e.g., Electronics, Clothing). | Strong Entity |
| **SUPPLIER** | Companies that supply products to WitleShop. | Strong Entity |
| **PRODUCT** | Items for sale, linked to one category and one supplier. | Strong Entity |
| **ORDER** | Purchases made by a registered customer (table named `Orders` in SQL, as ORDER is a reserved word). | Strong Entity |
| **ORDER_LINE** | Bridge entity resolving the M:N relationship between Orders and Products. | Associative Entity |
| **PAYMENT** | Payment records (1:1 with Orders). | Strong Entity |
| **DELIVERY** | Delivery records (1:1 with Orders). | Strong Entity |

## 🔗 Key Relationships & Cardinality
*   **1:M** – Customer to Address, Customer to Order, Category to Product, Supplier to Product, Address to Delivery.
*   **M:N** – Order to Product (resolved via the `ORDER_LINE` associative entity).
*   **1:1** – Order to Payment, Order to Delivery (enforced via `UNIQUE` constraints on the `OrderID` foreign keys).

## 🛠️ Key Design Decisions
*   **Surrogate Keys:** System-generated integers are used as primary keys for stability and compactness, rather than natural keys like email addresses.
*   **Price History:** `UnitPrice` is stored in `ORDER_LINE` so historical order totals are not affected by future product price changes.
*   **Data Integrity:** `CHECK` constraints are used for fixed lists (e.g., Order Status, Payment Methods).
*   **Security & Privacy:** The design accounts for the Protection of Personal Information Act (POPIA) by storing only necessary customer data.
*   **Delivery Address Ownership:** To ensure a delivery goes to the customer's own address, a database trigger compares the address's `CustomerID` with the order's `CustomerID` (see `schema.sql` and Section 11 of the documentation).
*   **Normalisation:** The design satisfies 3NF, with controlled denormalisation only for derived amounts (`LineTotal`, `TotalAmount`) to simplify reporting.

## 🚀 How to Use
1. Clone this repository.
2. Open the PDF document to view the full data dictionary, ERD, and detailed design justifications.
3. Run `schema.sql` in PostgreSQL to create the database structure.

## 📚 References
Chen, P.P. (1976) 'The entity-relationship model: toward a unified view of data', *ACM Transactions on Database Systems*, 1(1), pp. 9-36.

Codd, E.F. (1970) 'A relational model of data for large shared data banks', *Communications of the ACM*, 13(6), pp. 377-387.

Coronel, C. and Morris, S. (2019) *Database systems: design, implementation, and management*. 13th edn. Boston: Cengage Learning.

Elmasri, R. and Navathe, S.B. (2016) *Fundamentals of database systems*. 7th edn. Harlow: Pearson.

Silberschatz, A., Korth, H.F. and Sudarshan, S. (2019) *Database system concepts*. 7th edn. New York: McGraw-Hill Education.
