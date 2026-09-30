# مخطط الكيانات والعلاقات (ERD) - نظام القيد المزدوج والمخزون

## الكيانات (Entities)

### العميل (Customer)
- id (PK)
- name
- phone
- balance
- is_deleted (boolean, default false)
- row_version (integer, for optimistic locking)

### المورد (Supplier)
- id (PK)
- name
- phone
- balance
- is_deleted (boolean, default false)
- row_version (integer, for optimistic locking)

### الصنف (Item)
- id (PK)
- name
- barcode
- stock_quantity
- unit_price
- is_deleted (boolean, default false)
- row_version (integer, for optimistic locking)

### الفاتورة (Invoice - Sale/Purchase)
- id (PK)
- invoice_type (Sale / Purchase)
- customer_id (FK -> Customer.id) [Nullable]
- supplier_id (FK -> Supplier.id) [Nullable]
- total_amount
- created_at
- is_deleted (boolean, default false)
- row_version (integer, for optimistic locking)
- device_id (string)
- created_by_user (string)

### حركة المخزون (InventoryMovement)
- id (PK)
- invoice_id (FK -> Invoice.id)
- item_id (FK -> Item.id)
- type (In / Out)
- quantity
- is_deleted (boolean, default false)
- row_version (integer, for optimistic locking)
- device_id (string)

### قيد اليومية (JournalEntry - Double-Entry)
- id (PK)
- reference_id (FK -> Invoice.id / Receipt.id)
- account_name
- debit
- credit
- created_at
- is_deleted (boolean, default false)
- row_version (integer, for optimistic locking)
- device_id (string)
- created_by_user (string)

## العلاقات (Relationships)
- Customer [1] ---> [0..*] Invoice (مبيعات العميل)
- Supplier [1] ---> [0..*] Invoice (مشتريات المورد)
- Invoice [1] ---> [1..*] InventoryMovement (حركات الأصناف بالفاتورة)
- Item [1] ---> [0..*] InventoryMovement (أرصدة الصنف)
- Invoice [1] ---> [2..*] JournalEntry (القيود المزدوجة للفاتورة)
