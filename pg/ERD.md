# ERD — Диаграмма структуры БД (Mermaid)

Ниже приведена ERD-диаграмма для базы данных (`orders`, `shipments`, расширение для операций).

```mermaid
erDiagram
    ORDERS {
        bigint id PK
        varchar order_code
        varchar product
        integer quantity
        date due_date
        smallint priority
        varchar status
        timestamptz created_at
        timestamptz updated_at
    }

    SHIPMENTS {
        bigint id PK
        bigint order_id FK
        integer shipped_qty
        date shipped_at
        text note
        timestamptz created_at
    }

    OPERATIONS {
        bigint id PK
        varchar code
        varchar name
        bigint resource_id FK
        integer duration_minutes
    }

    RESOURCES {
        bigint id PK
        varchar code
        varchar name
        text description
    }

    ORDER_OPERATIONS {
        bigint id PK
        bigint order_id FK
        bigint operation_id FK
        integer sequence
        integer qty
    }

    ORDERS ||--o{ SHIPMENTS : has
    ORDERS ||--o{ ORDER_OPERATIONS : contains
    OPERATIONS ||--o{ ORDER_OPERATIONS : used_in
    RESOURCES ||--o{ OPERATIONS : provides
