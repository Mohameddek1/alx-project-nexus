# Multi-Business Finance System - Lucid ERD Format

## 📋 LUCID DIAGRAM INSTRUCTIONS

Copy and paste the entity definitions below into Lucid to create your ERD diagram.

---

## 🎯 ENTITIES FOR LUCID DIAGRAM

### 1. User Entity
```
User
├── id (PK)
├── username
├── email
├── first_name
├── last_name
├── date_joined
├── last_login
├── password
├── is_active
├── is_staff
└── is_superuser
```

### 2. Business Entity
```
Business
├── id (PK)
├── name
├── description
├── owner_id (FK → User.id)
├── currency
├── fiscal_year_start
├── default_language
├── created_at
└── updated_at
```

### 3. UserBusinessRole Entity
```
UserBusinessRole
├── id (PK)
├── user_id (FK → User.id)
├── business_id (FK → Business.id)
├── role (owner|admin|accountant|employee|viewer)
├── assigned_at
└── assigned_by_id (FK → User.id)
```

### 4. TransactionCategory Entity
```
TransactionCategory
├── id (PK)
├── business_id (FK → Business.id)
├── name
├── type (income|expense|both)
├── description
├── is_active
├── created_at
└── created_by_id (FK → User.id)
```

### 5. Transaction Entity
```
Transaction
├── id (PK)
├── business_id (FK → Business.id)
├── category_id (FK → TransactionCategory.id)
├── type (income|expense)
├── amount (decimal)
├── date
├── description
├── reference_number
├── created_by_id (FK → User.id)
├── created_at
├── updated_at
├── updated_by_id (FK → User.id)
├── is_deleted
├── deleted_at
└── deleted_by_id (FK → User.id)
```

### 6. AuditLog Entity
```
AuditLog
├── id (PK)
├── user_id (FK → User.id)
├── business_id (FK → Business.id)
├── action (create|update|delete|assign|remove)
├── entity_type (business|transaction|category|user_role)
├── entity_id
├── details (JSON)
├── timestamp
└── ip_address
```

### 7. InterBusinessTransaction Entity
```
InterBusinessTransaction
├── id (PK)
├── from_business_id (FK → Business.id)
├── to_business_id (FK → Business.id)
├── transaction_type (loan|transfer|shared_expense|investment|repayment)
├── amount (decimal)
├── date
├── due_date
├── purpose
├── category
├── priority (high|medium|low)
├── status (pending|completed|partially_paid|fully_paid|cancelled)
├── amount_paid (decimal)
├── notes
├── attachment
├── created_by_id (FK → User.id)
├── created_at
├── updated_at
├── is_deleted
├── deleted_at
└── deleted_by_id (FK → User.id)
```

### 8. InterBusinessBalance Entity
```
InterBusinessBalance
├── id (PK)
├── business_a_id (FK → Business.id)
├── business_b_id (FK → Business.id)
├── net_balance (decimal)
└── last_updated
```

### 9. RepaymentSchedule Entity
```
RepaymentSchedule
├── id (PK)
├── inter_transaction_id (FK → InterBusinessTransaction.id)
├── installment_number
├── due_date
├── amount_due (decimal)
├── amount_paid (decimal)
├── paid_date
├── is_paid
├── is_overdue
└── late_fee (decimal)
```

### 10. SharedExpense Entity
```
SharedExpense
├── id (PK)
├── name
├── total_amount (decimal)
├── expense_date
├── category
├── description
├── paid_by_business_id (FK → Business.id)
├── split_method (equal|percentage|custom)
├── created_by_id (FK → User.id)
└── created_at
```

### 11. SharedExpenseSplit Entity
```
SharedExpenseSplit
├── id (PK)
├── shared_expense_id (FK → SharedExpense.id)
├── business_id (FK → Business.id)
├── amount_owed (decimal)
├── amount_paid (decimal)
├── percentage (decimal)
├── is_settled
└── settled_date
```

---

## 🔗 RELATIONSHIPS FOR LUCID DIAGRAM

### One-to-Many Relationships
```
User (1) ────→ (∞) Business (owner)
User (1) ────→ (∞) UserBusinessRole
User (1) ────→ (∞) TransactionCategory (created_by)
User (1) ────→ (∞) Transaction (created_by)
User (1) ────→ (∞) AuditLog
User (1) ────→ (∞) InterBusinessTransaction (created_by)
User (1) ────→ (∞) SharedExpense (created_by)

Business (1) ────→ (∞) UserBusinessRole
Business (1) ────→ (∞) TransactionCategory
Business (1) ────→ (∞) Transaction
Business (1) ────→ (∞) AuditLog
Business (1) ────→ (∞) InterBusinessTransaction (from_business)
Business (1) ────→ (∞) InterBusinessTransaction (to_business)
Business (1) ────→ (∞) InterBusinessBalance (business_a)
Business (1) ────→ (∞) InterBusinessBalance (business_b)
Business (1) ────→ (∞) SharedExpense (paid_by_business)
Business (1) ────→ (∞) SharedExpenseSplit

TransactionCategory (1) ────→ (∞) Transaction

InterBusinessTransaction (1) ────→ (∞) RepaymentSchedule

SharedExpense (1) ────→ (∞) SharedExpenseSplit
```

### Many-to-Many Relationships (via Junction Tables)
```
User (∞) ──── UserBusinessRole ──── (∞) Business
Business (∞) ──── InterBusinessBalance ──── (∞) Business
```

### Unique Constraints
```
UserBusinessRole: (user, business) - UNIQUE
TransactionCategory: (business, name) - UNIQUE
InterBusinessBalance: (business_a, business_b) - UNIQUE
```

---

## 🎨 LUCID DIAGRAM CREATION STEPS

### Step 1: Create Entities
1. Open Lucid and create a new diagram
2. Add 11 entity boxes (rectangles) for each table
3. Label each entity with its name
4. Add attributes as bullet points inside each box

### Step 2: Add Relationships
1. Use arrows to connect entities
2. Label relationships with cardinality (1 → ∞)
3. Use different arrow styles for different relationship types

### Step 3: Add Constraints
1. Add note boxes for unique constraints
2. Add note boxes for important business rules
3. Color-code different types of relationships

### Step 4: Format and Style
1. Align entities in a logical layout
2. Use consistent colors for related entities
3. Add a legend explaining relationship types
4. Add title and description

---

## 📊 LUCID DIAGRAM LAYOUT SUGGESTION

```
┌─────────────┐     ┌─────────────┐
│    User     │─────│ UserBusiness│
│             │     │    Role     │
└─────────────┘     └─────────────┘
       │                   │
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│  Business   │◄────┤Transaction │
│             │     │ Category   │
└─────────────┘     └─────────────┘
       │                   │
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│ Transaction │     │  AuditLog   │
│             │     │             │
└─────────────┘     └─────────────┘
       │
       │
       ▼
┌─────────────┐     ┌─────────────┐
│InterBusiness│─────│InterBusiness│
│Transaction  │     │  Balance    │
└─────────────┘     └─────────────┘
       │                   │
       │                   │
       ▼                   ▼
┌─────────────┐     ┌─────────────┐
│Repayment    │     │ Shared      │
│ Schedule    │     │ Expense     │
└─────────────┘     └─────────────┘
                           │
                           │
                           ▼
                    ┌─────────────┐
                    │SharedExpense│
                    │   Split     │
                    └─────────────┘
```

---

## 🔧 LUCID FORMATTING TIPS

### Entity Styling
- **Header**: Bold, centered, background color
- **Primary Keys**: (PK) in bold, different color
- **Foreign Keys**: (FK → Table.field) in italics
- **Data Types**: In parentheses, smaller font

### Relationship Styling
- **One-to-Many**: Solid arrow (→)
- **Many-to-Many**: Dashed line with crows feet
- **Cardinality**: Numbers near connection points
- **Labels**: Above/below relationship lines

### Color Coding
- **User/Auth**: Blue theme
- **Business**: Green theme
- **Transactions**: Orange theme
- **Inter-business**: Purple theme
- **Audit/Logs**: Gray theme

### Additional Elements
- **Legend Box**: Explains symbols and colors
- **Title Box**: "Multi-Business Finance System ERD"
- **Notes Boxes**: For constraints and business rules
- **Version Box**: Current version and date

---

## 📋 COPY-PASTE READY ENTITIES

Use these formatted blocks to quickly create entities in Lucid:

**User Entity:**
```
User
• id (PK)
• username
• email
• first_name
• last_name
• date_joined
• last_login
• password
• is_active
• is_staff
• is_superuser
```

**Business Entity:**
```
Business
• id (PK)
• name
• description
• owner_id (FK → User.id)
• currency
• fiscal_year_start
• default_language
• created_at
• updated_at
```

**Transaction Entity:**
```
Transaction
• id (PK)
• business_id (FK → Business.id)
• category_id (FK → TransactionCategory.id)
• type (income|expense)
• amount (decimal)
• date
• description
• reference_number
• created_by_id (FK → User.id)
• created_at
• updated_at
• is_deleted
• deleted_at
• deleted_by_id (FK → User.id)
```

This format is optimized for easy copying into Lucid to create a professional ERD diagram.