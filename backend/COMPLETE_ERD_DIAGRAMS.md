# Multi-Business Finance System - Complete ERD Diagrams

## 🏗️ COMPREHENSIVE ENTITY RELATIONSHIP DIAGRAM (ERD)

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           MULTI-BUSINESS FINANCE SYSTEM                         │
│                               ENTITY RELATIONSHIP DIAGRAM                      │
├─────────────────────────────────────────────────────────────────────────────────┤
│                                                                               │
│  ┌─────────────────────────────────────────────────────────────────────────┐   │
│  │                           USER MANAGEMENT                               │   │
│  ├─────────────────────────────────────────────────────────────────────────┤   │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐      │   │
│  │  │      User       │────│ UserBusinessRole │────│    Business     │      │   │
│  │  │  (Django Auth)  │    │                 │    │                 │      │   │
│  │  │─────────────────│    │─────────────────│    │─────────────────│      │   │
│  │  │ id (PK)         │    │ id (PK)         │    │ id (PK)         │      │   │
│  │  │ username        │    │ user_id (FK)    │◄───│ name            │      │   │
│  │  │ email           │    │ business_id (FK)│────│ description      │      │   │
│  │  │ password        │    │ role            │    │ owner_id (FK)    │◄─────┘   │
│  │  │ is_active       │    │ assigned_at     │    │ currency         │          │
│  │  │ date_joined     │    │ assigned_by_id  │    │ fiscal_year_start│          │
│  │  │                 │    │ (FK)            │    │ default_language │          │
│  │  │                 │    └─────────────────┘    │ created_at       │          │
│  │  └─────────────────┘                           │ updated_at       │          │
│  │                                               └─────────────────┘          │
│  └─────────────────────────────────────────────────────────────────────────┘   │
│                                                                               │
│  ┌─────────────────────────────────────────────────────────────────────────┐   │
│  │                         TRANSACTION MANAGEMENT                          │   │
│  ├─────────────────────────────────────────────────────────────────────────┤   │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐      │   │
│  │  │Transaction      │────│   Transaction   │────│   AuditLog      │      │   │
│  │  │   Category      │    │                 │    │                 │      │   │
│  │  │─────────────────│    │─────────────────│    │─────────────────│      │   │
│  │  │ id (PK)         │    │ id (PK)         │    │ id (PK)         │      │   │
│  │  │ business_id (FK)│◄───│ business_id (FK)│    │ user_id (FK)    │◄─────┘   │
│  │  │ name            │    │ category_id (FK)│◄───│ business_id (FK)│          │
│  │  │ type            │    │ type            │    │ action          │          │
│  │  │ description     │    │ amount          │    │ entity_type     │          │
│  │  │ is_active       │    │ date            │    │ entity_id       │          │
│  │  │ created_at      │    │ description     │    │ details (JSON)  │          │
│  │  │ created_by_id   │    │ reference_number│    │ timestamp       │          │
│  │  │ (FK)            │    │ created_by_id   │    │ ip_address      │          │
│  │  │                 │    │ (FK)            │    └─────────────────┘          │
│  │  │                 │    │ created_at      │                                │
│  │  │                 │    │ updated_at      │                                │
│  │  │                 │    │ updated_by_id   │                                │
│  │  │                 │    │ (FK)            │                                │
│  │  │                 │    │ is_deleted      │                                │
│  │  │                 │    │ deleted_at      │                                │
│  │  │                 │    │ deleted_by_id   │                                │
│  │  │                 │    │ (FK)            │                                │
│  │  └─────────────────┘    └─────────────────┘                                │
│  └─────────────────────────────────────────────────────────────────────────┘   │
│                                                                               │
│  ┌─────────────────────────────────────────────────────────────────────────┐   │
│  │                    INTER-BUSINESS RELATIONSHIPS                         │   │
│  ├─────────────────────────────────────────────────────────────────────────┤   │
│  │  ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐      │   │
│  │  │InterBusiness    │────│InterBusiness    │────│Repayment        │      │   │
│  │  │Transaction      │    │  Balance        │    │Schedule         │      │   │
│  │  │─────────────────│    │─────────────────│    │─────────────────│      │   │
│  │  │ id (PK)         │    │ id (PK)         │    │ id (PK)         │      │   │
│  │  │ from_business_id│◄───│ business_a_id   │    │ inter_txn_id    │◄─────┘   │
│  │  │ (FK)            │────│ (FK)            │    │ (FK)            │          │
│  │  │ to_business_id  │────│ business_b_id   │    │ installment_num │          │
│  │  │ (FK)            │    │ (FK)            │    │ due_date        │          │
│  │  │ transaction_type│    │ net_balance     │    │ amount_due      │          │
│  │  │ amount          │    │ last_updated    │    │ amount_paid     │          │
│  │  │ date            │    └─────────────────┘    │ paid_date       │          │
│  │  │ due_date        │                           │ is_paid         │          │
│  │  │ purpose         │                           │ is_overdue      │          │
│  │  │ category        │                           │ late_fee        │          │
│  │  │ priority        │                           └─────────────────┘          │
│  │  │ status          │                                                        │
│  │  │ amount_paid     │                                                        │
│  │  │ notes           │                                                        │
│  │  │ attachment      │                                                        │
│  │  │ created_by_id   │                                                        │
│  │  │ (FK)            │                                                        │
│  │  │ created_at      │                                                        │
│  │  │ updated_at      │                                                        │
│  │  │ is_deleted      │                                                        │
│  │  │ deleted_at      │                                                        │
│  │  │ deleted_by_id   │                                                        │
│  │  │ (FK)            │                                                        │
│  │  └─────────────────┘                                                        │
│  └─────────────────────────────────────────────────────────────────────────┘   │
│                                                                               │
│  ┌─────────────────────────────────────────────────────────────────────────┐   │
│  │                        SHARED EXPENSE MANAGEMENT                        │   │
│  ├─────────────────────────────────────────────────────────────────────────┤   │
│  │  ┌─────────────────┐    ┌─────────────────┐                             │   │
│  │  │  SharedExpense  │────│SharedExpense    │                             │   │
│  │  │                 │    │  Split          │                             │   │
│  │  │─────────────────│    │─────────────────│                             │   │
│  │  │ id (PK)         │    │ id (PK)         │                             │   │
│  │  │ name            │    │ shared_exp_id   │◄────────────────────────────┘   │
│  │  │ total_amount    │    │ (FK)            │                                 │
│  │  │ expense_date    │    │ business_id (FK)│◄─────────────────────────────┘   │
│  │  │ category        │    │ amount_owed     │                                 │
│  │  │ description     │    │ amount_paid     │                                 │
│  │  │ paid_by_business│    │ percentage      │                                 │
│  │  │ (FK)            │    │ is_settled      │                                 │
│  │  │ split_method    │    │ settled_date    │                                 │
│  │  │ created_by_id   │    └─────────────────┘                                 │
│  │  │ (FK)            │                                                         │
│  │  │ created_at      │                                                         │
│  │  └─────────────────┘                                                         │
│  └─────────────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📋 RELATIONSHIP DETAILS

### **ONE-TO-MANY RELATIONSHIPS**
```
User → Business (owner)           : One user can own multiple businesses
User → UserBusinessRole           : One user can have multiple roles
Business → UserBusinessRole       : One business can have multiple users
Business → TransactionCategory    : One business can have multiple categories
Business → Transaction            : One business can have multiple transactions
Business → AuditLog               : One business can have multiple audit entries
User → TransactionCategory        : One user can create multiple categories
User → Transaction (created_by)   : One user can create multiple transactions
TransactionCategory → Transaction : One category can have multiple transactions
```

### **MANY-TO-MANY RELATIONSHIPS (via junction tables)**
```
Business ↔ User (via UserBusinessRole)
  - One user can access multiple businesses
  - One business can have multiple users
  - Each relationship has a specific role

Business ↔ Business (via InterBusinessTransaction)
  - Businesses can exchange money via loans/transfers
  - Tracked through InterBusinessTransaction table

Business ↔ Business (via InterBusinessBalance)
  - Net balance tracking between business pairs
  - Unique constraint on business pairs
```

### **SELF-REFERENTIAL RELATIONSHIPS**
```
InterBusinessBalance: business_a ↔ business_b
  - Tracks balance between any two businesses
  - business_a and business_b are both Business entities
```

---

## 🔗 KEY CONSTRAINTS & BUSINESS RULES

### **UNIQUE CONSTRAINTS**
```
UserBusinessRole: (user, business) - One role per user per business
TransactionCategory: (business, name) - Unique category names per business
InterBusinessBalance: (business_a, business_b) - One balance record per business pair
```

### **FOREIGN KEY CONSTRAINTS**
```
Business.owner → User.id (CASCADE)
UserBusinessRole.user → User.id (CASCADE)
UserBusinessRole.business → Business.id (CASCADE)
TransactionCategory.business → Business.id (CASCADE)
Transaction.business → Business.id (CASCADE)
Transaction.category → TransactionCategory.id (PROTECT)
AuditLog.user → User.id (SET_NULL)
AuditLog.business → Business.id (CASCADE)
```

### **BUSINESS RULES**
```
1. Soft Delete: Transactions use is_deleted flag instead of hard delete
2. Category Validation: Transaction type must match category type (income/expense/both)
3. Balance Updates: Inter-business transactions automatically update balances
4. Role Hierarchy: Owner > Admin > Accountant > Employee > Viewer
5. Audit Trail: All major actions are logged with user, timestamp, and details
6. Business Isolation: Users can only access businesses they're assigned to
```

---

## 📊 DATABASE STATISTICS

### **TABLE COUNT**: 11 main tables
### **RELATIONSHIPS**: 25+ foreign key relationships
### **INDEXES NEEDED**:
- Business.owner_id
- UserBusinessRole.user_id, business_id
- Transaction.business_id, category_id, date
- TransactionCategory.business_id
- AuditLog.business_id, timestamp
- InterBusinessTransaction.from_business_id, to_business_id
- InterBusinessBalance.business_a_id, business_b_id

### **EXPECTED DATA GROWTH**:
- **Users**: Moderate growth
- **Businesses**: Moderate growth per user
- **Transactions**: High growth (daily entries)
- **Audit Logs**: High growth (all actions logged)
- **Inter-business**: Moderate growth

---

## 🗂️ DATA FLOW DIAGRAM

```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   User      │────│Business Mgmt│────│Transaction │
│  Management │    │             │    │ Management │
└─────────────┘    └─────────────┘    └─────────────┘
       │                   │                   │
       │                   │                   │
       └───────────────────┼───────────────────┘
                           │
                    ┌─────────────┐
                    │Inter-Business│
                    │Transactions │
                    └─────────────┘
                           │
                    ┌─────────────┐
                    │  Analytics  │
                    │  & Reports  │
                    └─────────────┘
```

---

## 🎯 LUCID CHART COMPATIBLE ERD

### **Copy-Paste Ready Entity Definitions**

```
# User Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| username    | varchar(150)| NO   | UNI | NULL    |                |
| email       | varchar(254)| NO   | UNI | NULL    |                |
| password    | varchar(128)| NO   |     | NULL    |                |
| is_active   | tinyint(1)  | NO   |     | 1       |                |
| date_joined | datetime    | NO   |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# Business Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| name        | varchar(255)| NO   |     | NULL    |                |
| description | text        | YES  |     | NULL    |                |
| owner_id    | int(11)     | NO   | MUL | NULL    |                |
| currency    | varchar(3)  | NO   |     | USD     |                |
| fiscal_year_start| smallint| NO   |     | 1       |                |
| default_language| varchar(10)| NO |     | en      |                |
| created_at  | datetime    | NO   |     | NULL    |                |
| updated_at  | datetime    | NO   |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# UserBusinessRole Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| user_id     | int(11)     | NO   | MUL | NULL    |                |
| business_id | int(11)     | NO   | MUL | NULL    |                |
| role        | varchar(20) | NO   |     | NULL    |                |
| assigned_at | datetime    | NO   |     | NULL    |                |
| assigned_by_id| int(11)   | YES  | MUL | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# TransactionCategory Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| business_id | int(11)     | NO   | MUL | NULL    |                |
| name        | varchar(100)| NO   |     | NULL    |                |
| type        | varchar(10) | NO   |     | both    |                |
| description | text        | YES  |     | NULL    |                |
| is_active   | tinyint(1)  | NO   |     | 1       |                |
| created_at  | datetime    | NO   |     | NULL    |                |
| created_by_id| int(11)    | YES  | MUL | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# Transaction Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| business_id | int(11)     | NO   | MUL | NULL    |                |
| category_id | int(11)     | NO   | MUL | NULL    |                |
| type        | varchar(10) | NO   |     | NULL    |                |
| amount      | decimal(12,2)| NO  |     | NULL    |                |
| date        | date        | NO   |     | NULL    |                |
| description | text        | NO   |     | NULL    |                |
| reference_number| varchar(50)| YES|     | NULL    |                |
| created_by_id| int(11)    | NO   | MUL | NULL    |                |
| created_at  | datetime    | NO   |     | NULL    |                |
| updated_at  | datetime    | NO   |     | NULL    |                |
| updated_by_id| int(11)    | YES  | MUL | NULL    |                |
| is_deleted  | tinyint(1)  | NO   |     | 0       |                |
| deleted_at  | datetime    | YES  |     | NULL    |                |
| deleted_by_id| int(11)    | YES  | MUL | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# AuditLog Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| user_id     | int(11)     | YES  | MUL | NULL    |                |
| business_id | int(11)     | YES  | MUL | NULL    |                |
| action      | varchar(20) | NO   |     | NULL    |                |
| entity_type | varchar(20) | NO   |     | NULL    |                |
| entity_id   | int(11)     | NO   |     | NULL    |                |
| details     | json        | NO   |     | {}      |                |
| timestamp   | datetime    | NO   |     | NULL    |                |
| ip_address  | varchar(39) | YES  |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# InterBusinessTransaction Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| from_business_id| int(11)  | NO   | MUL | NULL    |                |
| to_business_id| int(11)    | NO   | MUL | NULL    |                |
| transaction_type| varchar(20)| NO |     | NULL    |                |
| amount      | decimal(12,2)| NO  |     | NULL    |                |
| date        | date        | NO   |     | NULL    |                |
| due_date    | date        | YES  |     | NULL    |                |
| purpose     | varchar(255)| NO   |     | NULL    |                |
| category    | varchar(100)| YES  |     | NULL    |                |
| priority    | varchar(10) | NO   |     | medium  |                |
| status      | varchar(20) | NO   |     | pending |                |
| amount_paid | decimal(12,2)| NO  |     | 0       |                |
| notes       | text        | YES  |     | NULL    |                |
| attachment  | varchar(100)| YES  |     | NULL    |                |
| created_by_id| int(11)    | YES  | MUL | NULL    |                |
| created_at  | datetime    | NO   |     | NULL    |                |
| updated_at  | datetime    | NO   |     | NULL    |                |
| is_deleted  | tinyint(1)  | NO   |     | 0       |                |
| deleted_at  | datetime    | YES  |     | NULL    |                |
| deleted_by_id| int(11)    | YES  | MUL | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# InterBusinessBalance Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| business_a_id| int(11)    | NO   | MUL | NULL    |                |
| business_b_id| int(11)    | NO   | MUL | NULL    |                |
| net_balance | decimal(12,2)| NO  |     | 0       |                |
| last_updated| datetime    | NO   |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# RepaymentSchedule Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| inter_transaction_id| int(11)| NO| MUL | NULL    |                |
| installment_number| int(11) | NO  |     | NULL    |                |
| due_date    | date        | NO   |     | NULL    |                |
| amount_due  | decimal(12,2)| NO  |     | NULL    |                |
| amount_paid | decimal(12,2)| NO  |     | 0       |                |
| paid_date   | date        | YES  |     | NULL    |                |
| is_paid     | tinyint(1)  | NO   |     | 0       |                |
| is_overdue  | tinyint(1)  | NO   |     | 0       |                |
| late_fee    | decimal(10,2)| NO  |     | 0       |                |
+-------------+-------------+------+-----+---------+----------------+

# SharedExpense Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| name        | varchar(255)| NO   |     | NULL    |                |
| total_amount| decimal(12,2)| NO  |     | NULL    |                |
| expense_date| date        | NO   |     | NULL    |                |
| category    | varchar(100)| NO   |     | NULL    |                |
| description | text        | YES  |     | NULL    |                |
| paid_by_business_id| int(11)| NO | MUL | NULL    |                |
| split_method| varchar(20) | NO   |     | equal   |                |
| created_by_id| int(11)    | YES  | MUL | NULL    |                |
| created_at  | datetime    | NO   |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+

# SharedExpenseSplit Entity
+-------------+-------------+------+-----+---------+----------------+
| Field       | Type        | Null | Key | Default | Extra          |
+-------------+-------------+------+-----+---------+----------------+
| id          | int(11)     | NO   | PRI | NULL    | auto_increment |
| shared_expense_id| int(11) | NO  | MUL | NULL    |                |
| business_id | int(11)     | NO   | MUL | NULL    |                |
| amount_owed | decimal(12,2)| NO  |     | NULL    |                |
| amount_paid | decimal(12,2)| NO  |     | 0       |                |
| percentage  | decimal(5,2)| YES  |     | NULL    |                |
| is_settled  | tinyint(1)  | NO   |     | 0       |                |
| settled_date| date        | YES  |     | NULL    |                |
+-------------+-------------+------+-----+---------+----------------+
```

This ERD provides a complete visual representation of your multi-business finance system database design, ready for implementation with Django ORM.