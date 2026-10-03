# 🛒 FreshMart Grocery - Full-Stack Quick-Commerce Platform

FreshMart is a modern full-stack quick-commerce grocery and essentials delivery web application. The platform provides an intuitive shopping experience for customers, a dedicated back-office portal for store operators, and a scalable RESTful backend handling payments, order lifecycles, and automated notifications.

---

## 📑 Table of Contents

- [Overview](#-overview)
- [Architecture](#-architecture)
- [Key Features](#-key-features)
  - [Customer Storefront (`frontend`)](#1-customer-storefront-frontend)
  - [Admin Management Portal (`admin`)](#2-admin-management-portal-admin)
  - [Backend & APIs (`backend`)](#3-backend-services--apis-backend)
- [Tech Stack](#-tech-stack)
- [Directory Structure](#-directory-structure)
- [Environment Variables](#-environment-variables)
- [Getting Started](#-getting-started)
  - [Prerequisites](#prerequisites)
  - [Installation & Setup](#installation--setup)
- [API Overview](#-api-overview)
- [Order & Payment Workflow](#-order--payment-workflow)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🌟 Overview

FreshMart is designed for fast, frictionless grocery ordering and instant fulfillment tracking:
- **Customer Storefront**: Allows users to discover products across curated categories, search real-time inventory, manage multiple delivery addresses, and checkout using Cash on Delivery (COD) or Stripe.
- **Admin Dashboard**: Equips store managers with live business metrics (revenue, order counts, status breakdowns), real-time order progression controls, and full product/category CRUD capabilities.
- **Backend Architecture**: Modular controllers handling secure authentication with Brevo OTP verification, Stripe Checkout integration, and MongoDB-backed order processing.

---

## 🏗 Architecture

FreshMart is structured as a multi-tier workspace:

```
FreshMart_App/
├── frontend/   # Customer-facing shopping application (React 19 + Vite)
├── admin/      # Back-office store administration dashboard (React 19 + Vite)
└── backend/    # Core business logic, controllers, and email notification utilities
```

---

## 🚀 Key Features

### 1. Customer Storefront (`frontend`)
- **Product Discovery & Search**:
  - Category-based catalog browsing (Groceries, Hookah, Daily Staples, etc.).
  - Instant product search filtering by name, company, and category.
  - Interactive promotional banners and hero showcases.
- **Cart & Dynamic Pricing**:
  - Live cart drawer/page with increment, decrement, and item removal.
  - Automatic subtotal calculations and configured delivery fee handling.
- **Address & Checkout**:
  - Manage and select saved delivery addresses.
  - Dual checkout options: **Cash on Delivery (COD)** and **Online Stripe Payment**.
- **Customer Accounts & Security**:
  - User signup and secure login with JSON Web Tokens (JWT).
  - Brevo-powered One-Time Password (OTP) email verification.
  - Password recovery (Forgot Password / OTP-based Reset Password).
- **Order Tracking**:
  - Real-time customer order history and itemized receipts.
  - Instant redirect verification for Stripe checkout sessions.

---

### 2. Admin Management Portal (`admin`)
- **Executive Analytics Dashboard**:
  - Real-time revenue aggregation (differentiates COD collected upon delivery vs. paid Stripe transactions).
  - High-level KPIs: Total Orders, Active Orders, Delivered Orders, Cancelled Orders, Total Products, and Categories.
- **Order Processing Pipeline**:
  - Paginated orders table with real-time text search (customer name, email, phone number).
  - Quick status filtering (`All`, `Processing`, `On the way`, `Delivered`, `Cancelled`).
  - Order details modal showing item breakdowns, pricing, and shipping addresses.
  - Status updates and automated payment reconciliation.
  - Cancellation confirmation and safety modals.
- **Catalog & Inventory Management**:
  - Add, update, and remove products with details: size, brand/company, selling price, MRP/old price, and product images.
  - Manage product categories with Cloudinary image hosting.
  - Specialized catalog management (e.g., Hookah listings and items).

---

### 3. Backend Services & APIs (`backend`)
- **Authentication & Authorization**:
  - Bcrypt password hashing.
  - 10-minute expiring hashed OTPs for email verification and password resets.
  - JWT generation and payload verification.
- **Stripe Checkout Integration**:
  - Dynamic line-item session creation with custom order metadata.
  - Server-side payment verification and double-charge prevention.
- **Email Notifications**:
  - Brevo (Sendinblue) transactional email templates for OTP verification and password recovery.
- **Database Operations**:
  - Mongoose models with populated relationships (`User`, `Order`, `Product`, `Category`, `Address`).
  - Automatic category sync cascading across associated products.

---

## 💻 Tech Stack

### Frontend & Admin
- **Framework**: [React 19](https://react.dev/)
- **Build Tool**: [Vite](https://vitejs.dev/)
- **Styling**: [Tailwind CSS v4](https://tailwindcss.com/)
- **Routing**: [React Router v7](https://reactrouter.com/)
- **Icons**: [Lucide React](https://lucide.dev/)

### Backend & Cloud Services
- **Runtime**: [Node.js](https://nodejs.org/) (ES Modules)
- **Database**: [MongoDB](https://www.mongodb.com/) via [Mongoose](https://mongoosejs.com/)
- **Security**: [Bcrypt](https://www.npmjs.com/package/bcrypt), [JSON Web Tokens (JWT)](https://jwt.io/)
- **Payments**: [Stripe API](https://stripe.com/)
- **Media Storage**: [Cloudinary](https://cloudinary.com/)
- **Transactional Emails**: [Brevo](https://www.brevo.com/) (formerly Sendinblue)

---

## 📁 Directory Structure

```text
FreshMart_App/
├── admin/                         # Admin Dashboard Application
│   ├── public/                    # Static assets & icons
│   ├── src/
│   │   ├── api/                   # Admin API client services
│   │   ├── assets/                # Logos, dummy styles & UI helpers
│   │   ├── components/
│   │   │   ├── AddHookahPage/     # Hookah product creation form
│   │   │   ├── AddItemsPage/      # General product & category creation
│   │   │   ├── DashboardPage/     # KPI dashboard, orders table & modal
│   │   │   ├── HookahListPage/    # Hookah catalog management
│   │   │   └── ItemListPage/      # Inventory list and filters
│   │   ├── App.jsx                # Admin route definitions
│   │   └── main.jsx               # React DOM entry point
│   ├── package.json
│   └── vite.config.js
│
├── backend/                       # Server-side Business Logic
│   ├── controllers/
│   │   ├── adminController.js     # Dashboard revenue & stats aggregation
│   │   ├── adminOrderController.js# Order search, pagination & status edits
│   │   ├── categoryController.js  # Category CRUD & Cloudinary cleanup
│   │   ├── paymentController.js   # Stripe checkout session & verification
│   │   └── userController.js      # Auth, OTP verification & password reset
│   └── utils/
│       └── sendEmail.js           # Brevo transactional email triggers
│
├── frontend/                      # Customer Web Application
│   ├── public/                    # Static assets & icons
│   ├── src/
│   │   ├── api/                   # Client-side API integration
│   │   ├── assets/                # Product images, SVGs & mock styles
│   │   ├── components/
│   │   │   ├── AddressPage/       # Delivery address management
│   │   │   ├── Banner/            # Hero carousel & promotional banners
│   │   │   ├── CartPage/          # Cart drawer & checkout trigger
│   │   │   ├── Footer/            # Storefront footer & links
│   │   │   ├── HookahPage/        # Specialty section
│   │   │   ├── LoginPage/         # User login form
│   │   │   ├── Navbar/            # Navigation, search bar & cart count
│   │   │   ├── OrdersPage/        # Customer order tracking
│   │   │   ├── PaymentReturnPage/ # Stripe payment success/cancel return
│   │   │   ├── ProductItemsPage/  # Category product listing
│   │   │   ├── SearchResults/     # Query-based search results
│   │   │   └── SignupPage/        # New user registration
│   │   ├── pages/
│   │   │   └── auth/              # Email verification & password recovery
│   │   ├── App.jsx                # Main application routes
│   │   └── main.jsx               # React DOM entry point
│   ├── package.json
│   └── vite.config.js
│
└── README.md                      # Project documentation
```

---

## ⚙️ Environment Variables

Create a `.env` file in your backend server configuration with the following variables:

```env
# Server Configuration
PORT=5000
NODE_ENV=development
CLIENT_URL=http://localhost:5173

# Database
MONGO_URI=mongodb+srv://<username>:<password>@cluster.mongodb.net/freshmart

# Security
JWT_SECRET=your_jwt_secret_key_here

# Stripe Payments
STRIPE_SECRET_KEY=sk_test_your_stripe_secret_key

# Brevo (Sendinblue) Email Service
BREVO_API_KEY=your_brevo_api_key
SENDER_NAME=FreshMart Grocery
SENDER_EMAIL=no-reply@freshmart.com

# Cloudinary (Media Storage)
CLOUDINARY_CLOUD_NAME=your_cloud_name
CLOUDINARY_API_KEY=your_cloudinary_api_key
CLOUDINARY_API_SECRET=your_cloudinary_api_secret
```

---

## 🏁 Getting Started

### Prerequisites
- [Node.js](https://nodejs.org/) (v18.0.0 or higher recommended)
- [npm](https://www.npmjs.com/) or [yarn](https://yarnpkg.com/) / [pnpm](https://pnpm.io/)
- [MongoDB](https://www.mongodb.com/) instance (local or Atlas cluster)

---

### Installation & Setup

#### 1. Clone the repository
```bash
git clone https://github.com/your-username/FreshMart_App.git
cd FreshMart_App
```

#### 2. Run the Frontend (Customer App)
```bash
cd frontend
npm install
npm run dev
```
The customer app will start at `http://localhost:5173`.

#### 3. Run the Admin Dashboard
```bash
cd ../admin
npm install
npm run dev
```
The admin portal will start (typically at `http://localhost:5174` or the next available port).

#### 4. Run the Backend API
```bash
cd ../backend
npm install
npm start # or npm run dev with nodemon
```

---

## 📡 API Overview

### 🔐 Authentication (`/api/user`)
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/user/login` | Authenticate customer with email and password |
| `POST` | `/api/user/resend-otp` | Generate and email a new verification OTP |
| `POST` | `/api/user/forgot-password` | Send password reset OTP to user's email |
| `POST` | `/api/user/reset-password` | Validate OTP and set new password |
| `GET` | `/api/user/profile` | Retrieve authenticated user profile |

### 💳 Payments & Checkout (`/api/payment`)
| Method | Endpoint | Description |
|---|---|---|
| `POST` | `/api/payment/checkout-session` | Create Stripe checkout session with line items |
| `POST` | `/api/payment/verify` | Verify completed Stripe session & save order |

### 📊 Admin Operations (`/api/admin`)
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/admin/dashboard` | Fetch revenue, metrics, and recent orders |
| `GET` | `/api/admin/orders` | Get paginated orders with search and status filters |
| `GET` | `/api/admin/orders/:id` | Fetch detailed single order information |
| `PUT` | `/api/admin/orders/:id/status` | Update order status and reconcile payment |
| `DELETE` | `/api/admin/orders/:id` | Delete an order record |

### 🏷️ Categories & Products (`/api/categories`, `/api/products`)
| Method | Endpoint | Description |
|---|---|---|
| `GET` | `/api/categories` | Retrieve list of all categories |
| `GET` | `/api/categories/:id` | Retrieve single category details |
| `PUT` | `/api/categories/:id` | Update category & sync associated products |
| `DELETE` | `/api/categories/:id` | Delete category & delete Cloudinary image |

---

## 🔄 Order & Payment Workflow

```
Customer Cart
     │
     ├── Selects "Cash on Delivery" ──► Order Created (`Pending` payment)
     │                                           │
     │                                           ▼
     │                             Admin Marks "Delivered" ──► Order Marked `Paid`
     │
     └── Selects "Stripe Online" ───► Redirects to Stripe Checkout
                                                 │
                                                 ▼
                                     Stripe Session Succeeded
                                                 │
                                                 ▼
                                      Order Created as `Paid`
                                                 │
                                                 ▼
                                     Admin Dispatches Order (`On the way`)
```

---

## 🤝 Contributing

Contributions are always welcome!
1. Fork the Project.
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`).
3. Commit your Changes (`git commit -m 'Add some AmazingFeature'`).
4. Push to the Branch (`git push origin feature/AmazingFeature`).
5. Open a Pull Request.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
