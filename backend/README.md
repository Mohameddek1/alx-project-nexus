# Multi-Business Finance Management System

A comprehensive Django-based financial management platform that enables users to manage multiple businesses with advanced transaction tracking, inter-business operations, and detailed analytics.

## 🚀 Features

### 🔐 Authentication & User Management
- JWT-based authentication with secure login/logout
- User registration and profile management
- Token refresh functionality

### 🏢 Multi-Business Support
- Create and manage multiple businesses
- Role-based access control (Owner, Admin, Staff)
- Business-specific permissions and data isolation

### 💰 Transaction Management
- Income and expense tracking
- Customizable categories (income, expense, both)
- Advanced filtering by date, amount, category, and type
- Reference number support for external system integration

### 🔄 Inter-Business Operations
- **Loans**: Track loans between businesses with interest rates and due dates
- **Transfers**: Simple money transfers between businesses
- **Repayments**: Partial and full loan repayments with automatic balance updates
- **Shared Expenses**: Split expenses across multiple businesses

### 📊 Analytics & Reporting
- Financial summaries with date range filtering
- Category-wise breakdowns
- Cash flow analysis
- Inter-business balance tracking
- Comprehensive audit logs

### 🛡️ Security & Compliance
- Role-based permissions (Staff can only add income)
- Soft delete for transactions (audit trail preservation)
- Complete audit logging for all operations
- Business data isolation

## 🛠️ Tech Stack

- **Backend**: Django 5.2.4 + Django REST Framework 3.16.0
- **Authentication**: JWT (Simple JWT)
- **Database**: SQLite (development) / PostgreSQL (production)
- **API**: RESTful with comprehensive error handling
- **Permissions**: Custom role-based access control
- **Documentation**: Comprehensive API documentation

## 📋 Prerequisites

- Python 3.8+
- pip (Python package manager)
- Virtual environment (recommended)

## 🚀 Installation & Setup

### 1. Clone the Repository
```bash
git clone https://github.com/Mohameddek1/multi_bussiness_finance.git
cd multi_bussiness_finance/backend
```

### 2. Create Virtual Environment
```bash
python -m venv env
# On Windows:
env\Scripts\activate
# On macOS/Linux:
source env/bin/activate
```

### 3. Install Dependencies
```bash
pip install -r requirements.txt
```

### 4. Database Setup
```bash
# Run migrations
python manage.py makemigrations
python manage.py migrate
```

### 5. Create Superuser (Optional)
```bash
python manage.py createsuperuser
```

### 6. Run Development Server
```bash
python manage.py runserver
```

The API will be available at: `http://localhost:8000/api/`

## 📖 API Documentation

Complete API documentation is available in [`API_DOCUMENTATION.md`](./API_DOCUMENTATION.md) which includes:

- **21 comprehensive endpoints** with examples
- Request/response formats
- Authentication requirements
- Error handling
- Postman collection examples
- Testing scenarios

### Quick Start Endpoints:

#### Authentication
```bash
# Register a new user
POST /api/auth/register/

# Login
POST /api/auth/login/

# Get user profile
GET /api/auth/user/
```

#### Business Management
```bash
# Create a business
POST /api/businesses/

# List user's businesses
GET /api/businesses/

# Assign user to business
POST /api/businesses/{business_id}/assign-user/
```

#### Transactions
```bash
# Create default categories
POST /api/businesses/{business_id}/categories/create-defaults/

# Create transaction
POST /api/businesses/{business_id}/transactions/

# Get financial summary
GET /api/businesses/{business_id}/summary/
```

## 🏗️ Project Structure

```
backend/
├── API_DOCUMENTATION.md          # Complete API documentation
├── manage.py                     # Django management script
├── db.sqlite3                    # SQLite database
├── backend/                      # Main Django project
│   ├── settings.py              # Project settings
│   ├── urls.py                  # Main URL configuration
│   └── wsgi.py                  # WSGI configuration
├── auth_api/                     # Authentication app
│   ├── models.py                # User models
│   ├── views.py                 # Auth endpoints
│   ├── serializers.py           # Auth serializers
│   └── urls.py                  # Auth URLs
├── business_api/                 # Business management app
│   ├── models.py                # Business & UserRole models
│   ├── views.py                 # Business CRUD operations
│   ├── serializers.py           # Business serializers
│   └── urls.py                  # Business URLs
├── transaction_api/              # Transaction management app
│   ├── models.py                # Transaction, Category, Audit models
│   ├── views.py                 # Transaction endpoints
│   ├── serializers.py           # Transaction serializers
│   ├── permissions.py           # Custom permissions
│   └── urls.py                  # Transaction URLs
└── env/                         # Virtual environment
```

## 🎯 Key Features Explained

### Multi-Business Architecture
Unlike traditional finance apps that support only one business, this system allows users to:
- Own and manage multiple businesses
- Switch between businesses seamlessly
- Maintain separate financial data for each business
- Share resources between businesses (loans, transfers)

### Advanced Transaction System
- **Categories**: Flexible categorization (income/expense/both)
- **Filtering**: Powerful filtering by date, amount, category, type
- **Soft Delete**: Preserve audit trails while hiding deleted transactions
- **Reference Numbers**: Integration with external systems

### Inter-Business Operations
- **Loans**: Interest-bearing loans with repayment schedules
- **Transfers**: Simple money movements between businesses
- **Balance Tracking**: Automatic balance updates and reconciliation
- **Audit Trail**: Complete history of all inter-business operations

### Role-Based Security
- **Owner**: Full control, can delete business
- **Admin**: Manage users and transactions
- **Staff**: Limited access (income transactions only)
- **Business Isolation**: Users only see their authorized businesses

## 🧪 Testing

### Using Postman
1. Import the collection from `API_DOCUMENTATION.md`
2. Set environment variables:
   - `base_url`: `http://localhost:8000/api`
   - `token`: Your JWT token
   - `business_id`: Your business ID

### Manual Testing
```bash
# Test server is running
curl http://localhost:8000/api/auth/user/ \
  -H "Authorization: Bearer YOUR_JWT_TOKEN"
```

## 🔧 Configuration

### Environment Variables
Create a `.env` file in the backend directory:
```env
DEBUG=True
SECRET_KEY=your-secret-key-here
DATABASE_URL=sqlite:///db.sqlite3
JWT_SECRET_KEY=your-jwt-secret
```

### Database Configuration
The project uses SQLite by default. For production, update `settings.py`:
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'your_db_name',
        'USER': 'your_db_user',
        'PASSWORD': 'your_db_password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature-name`
3. Make your changes and test thoroughly
4. Commit your changes: `git commit -am 'Add feature'`
5. Push to the branch: `git push origin feature-name`
6. Submit a pull request

### Development Guidelines
- Follow Django best practices
- Write comprehensive tests
- Update API documentation for new endpoints
- Use meaningful commit messages
- Maintain code quality and security

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 📞 Support

For questions or issues:
1. Check the [API Documentation](./API_DOCUMENTATION.md)
2. Review existing issues on GitHub
3. Create a new issue with detailed information

## 🎉 Acknowledgments

- Django REST Framework for the powerful API toolkit
- Simple JWT for authentication
- The Django community for excellent documentation and support

---

**Built with ❤️ using Django & Django REST Framework**

*This project demonstrates advanced Django development with multi-tenant architecture, complex business logic, and production-ready API design.*</content>
<parameter name="filePath">c:\Users\mohadeq\Desktop\mutli_business\backend\README.md