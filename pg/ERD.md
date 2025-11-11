# ERD — Диаграмма структуры БД (Mermaid)

Ниже приведена простая ERD-диаграмма для базы данных, используемой в тестовых заданиях (`orders`, `shipments`).

---

```mermaid
erDiagram
    ORDERS {
        bigint id PK "Primary key"
        varchar order_code "unique order code"
        varchar product "product name"
        integer quantity "ordered quantity"
        date due_date "due date"
        smallint priority "1..5"
        varchar status "pending/partial/completed"
        timestamptz created_at
        timestamptz updated_at
    }

    SHIPMENTS {
        bigint id PK "Primary key"
        bigint order_id FK "FK -> orders.id"
        integer shipped_qty "shipped quantity"
        date shipped_at "shipment date"
        text note
        timestamptz created_at
    }

    OPERATIONS {
        bigint id PK "Primary key"
        varchar code "operation code"
        varchar name
        bigint resource_id FK "FK -> resources.id"
        integer duration_minutes
    }

    RESOURCES {
        bigint id PK "Primary key"
        varchar code "resource code"
        varchar name
        text description
    }

    ORDER_OPERATIONS {
        bigint id PK "Primary key"
        bigint order_id FK "FK -> orders.id"
        bigint operation_id FK "FK -> operations.id"
        integer sequence
        integer qty
    }

    -- отношения
    ORDERS ||--o{ SHIPMENTS : "has >"
    ORDERS ||--o{ ORDER_OPERATIONS : "contains >"
    OPERATIONS ||--o{ ORDER_OPERATIONS : "used in >"
    RESOURCES ||--o{ OPERATIONS : "provides >"
