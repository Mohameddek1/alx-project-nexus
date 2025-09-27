# Multi-Business Finance System - Entity Relationship Diagram (ERD)

## 📊 Database Schema Overview

```
┌─────────────────────────────────────────────────────────────────────────────────┐
│                           MULTI-BUSINESS FINANCE SYSTEM                         │
│                               DATABASE SCHEMA                                  │
└─────────────────────────────────────────────────────────────────────────────────┘
```

## 🏗️ ENTITIES & RELATIONSHIPS

### 1. USER MANAGEMENT
```
┌─────────────────┐
│     User        │  ← Django's built-in User model
│─────────────────│
│ id (PK)         │
│ username        │
│ email           │
│ first_name      │
│ last_name       │
│ date_joined     │
│ last_login      │
│ password        │
│ is_active       │
│ is_staff        │
│ is_superuser    │
└─────────────────┘
```

### 2. BUSINESS MANAGEMENT
```
┌─────────────────┐           ┌─────────────────────┐
│    Business     │           │ UserBusinessRole    │
│─────────────────│           │─────────────────────│
│ id (PK)         │◄──────────┤ id (PK)              │
│ name            │           │ user_id (FK)         │
│ description     │           │ business_id (FK)     │
│ owner_id (FK)   │──────────►│ role                 │
│ currency        │           │ assigned_at          │
│ fiscal_year_start│          │ assigned_by_id (FK)  │
│ default_language│           └─────────────────────┘
│ created_at      │                    │
│ updated_at      │                    │
└─────────────────┘                    │
          │                            │
          │                            │
          │  ┌─────────────────────┐   │
          │  │   ROLE TYPES        │   │
          │  │─────────────────────│   │
          │  │ • owner             │   │
          │  │ • admin             │   │
          │  │ • accountant        │   │
          │  │ • employee          │   │
          │  │ • viewer            │   │
          │  └─────────────────────┘   │
          │                            │
          └────────────────────────────┘
```

### 3. TRANSACTION MANAGEMENT
```
┌─────────────────────┐           ┌─────────────────┐
│ TransactionCategory │           │   Transaction   │
│─────────────────────│           │─────────────────│
│ id (PK)             │◄──────────┤ id (PK)          │
│ business_id (FK)    │           │ business_id (FK) │
│ name                │           │ category_id (FK) │
│ type                │           │ type             │
│ description         │           │ amount           │
│ is_active           │           │ date             │
│ created_at          │           │ description      │
│ created_by_id (FK)  │           │ reference_number │
└─────────────────────┘           │ created_by_id (FK)│
          │                       │ created_at       │
          │                       │ updated_at       │
          │                       │ updated_by_id (FK)│
          │                       │ is_deleted       │
          │                       │ deleted_at       │
          │                       │ deleted_by_id (FK)│
          │                       └─────────────────┘
          │
          │  ┌─────────────────────┐
          │  │  CATEGORY TYPES     │
          │  │─────────────────────│
          │  │ • income            │
          │  │ • expense           │
          │  │ • both              │
          │  └─────────────────────┘
```

### 4. AUDIT & LOGGING
```
┌─────────────────┐
│    AuditLog     │
│─────────────────│
│ id (PK)         │
│ user_id (FK)    │
│ business_id (FK)│
│ action          │
│ entity_type     │
│ entity_id       │
│ details (JSON)  │
│ timestamp       │
│ ip_address      │
└─────────────────┘

ACTION TYPES: create, update, delete, assign, remove
ENTITY TYPES: business, transaction, category, user_role
```

### 5. INTER-BUSINESS TRANSACTIONS
```
┌─────────────────────────┐           ┌─────────────────────┐
│ InterBusinessTransaction│           │ InterBusinessBalance│
│─────────────────────────┤           │─────────────────────│
│ id (PK)                 │           │ id (PK)              │
│ from_business_id (FK)   │◄──────────┤ business_a_id (FK)   │
│ to_business_id (FK)     │──────────►│ business_b_id (FK)   │
│ transaction_type        │           │ net_balance          │
│ amount                  │           │ last_updated         │
│ date                    │           └─────────────────────┘
│ due_date                │                    │
│ purpose                 │                    │
│ category                │                    │
│ priority                │                    │
│ status                  │                    │
│ amount_paid             │                    │
│ notes                   │                    │
│ attachment              │                    │
│ created_by_id (FK)      │                    │
│ created_at              │                    │
│ updated_at              │                    │
│ is_deleted              │                    │
│ deleted_at              │                    │
│ deleted_by_id (FK)      │                    │
└─────────────────────────┘                    │
          │                                   │
          │                                   │
          │  ┌─────────────────────┐          │
          │  │ TRANSACTION TYPES   │          │
          │  │─────────────────────│          │
          │  │ • loan              │          │
          │  │ • transfer          │          │
          │  │ • shared_expense    │          │
          │  │ • investment        │          │
          │  │ • repayment         │          │
          │  └─────────────────────┘          │
          │                                   │
          │  ┌─────────────────────┐          │
          │  │     STATUSES        │          │
          │  │─────────────────────│          │
          │  │ • pending           │          │
          │  │ • completed         │          │
          │  │ • partially_paid    │          │
          │  │ • fully_paid        │          │
          │  │ • cancelled         │          │
          │  └─────────────────────┘          │
          │                                   │
          └───────────────────────────────────┘
```

### 6. REPAYMENT MANAGEMENT
```
┌─────────────────────────┐
│   RepaymentSchedule     │
│─────────────────────────┤
│ id (PK)                 │
│ inter_transaction_id (FK)│◄──┐
│ installment_number      │   │
│ due_date                │   │
│ amount_due              │   │
│ amount_paid             │   │
│ paid_date               │   │
│ is_paid                 │   │
│ is_overdue              │   │
│ late_fee                │   │
└─────────────────────────┘   │
                              │
                              │
┌─────────────────────────┐   │
│ InterBusinessTransaction│───┘
│─────────────────────────┤
│ id (PK)                 │
│ ...                     │
└─────────────────────────┘
```

### 7. SHARED EXPENSES
```
┌─────────────────┐           ┌─────────────────────┐
│  SharedExpense  │           │ SharedExpenseSplit  │
│─────────────────│           │─────────────────────│
│ id (PK)         │◄──────────┤ id (PK)              │
│ name            │           │ shared_expense_id (FK)│
│ total_amount    │           │ business_id (FK)     │
│ expense_date    │           │ amount_owed          │
│ category        │           │ amount_paid          │
│ description     │           │ percentage           │
│ paid_by_business│           │ is_settled           │
│ split_method    │           │ settled_date         │
│ created_by_id   │           └─────────────────────┘
│ created_at      │
└─────────────────┘
          │
          │  ┌─────────────────────┐
          │  │  SPLIT METHODS      │
          │  │─────────────────────│
          │  │ • equal             │
          │  │ • percentage        │
          │  │ • custom            │
          │  └─────────────────────┘
```

---

## 🔗 RELATIONSHIP DETAILS

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

## 📋 KEY CONSTRAINTS & BUSINESS RULES

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

## 🔧 IMPLEMENTATION NOTES

### **Django ORM Implementation**
```python
# Example: Business Model
class Business(models.Model):
    name = models.CharField(max_length=255)
    owner = models.ForeignKey(User, on_delete=models.CASCADE)
    currency = models.CharField(max_length=3, default='USD')
    # ... other fields

# Example: Many-to-Many via Junction Table
class UserBusinessRole(models.Model):
    user = models.ForeignKey(User, on_delete=models.CASCADE)
    business = models.ForeignKey(Business, on_delete=models.CASCADE)
    role = models.CharField(max_length=20, choices=ROLE_CHOICES)

    class Meta:
        unique_together = ('user', 'business')
```

### **Migration Strategy**
```bash
# Initial migration
python manage.py makemigrations

# Apply migrations
python manage.py migrate

# Create superuser
python manage.py createsuperuser
```

This ERD represents a comprehensive multi-tenant finance system with proper data relationships, constraints, and business logic implementation.