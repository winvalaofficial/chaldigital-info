# Chaldigital — Affiliate & Course Platform

A full-featured **affiliate marketing and digital course platform** built with Laravel. The system supports package sales, multi-level referrals, multiple payment gateways, multi-currency checkout, and a complete admin and affiliate dashboard.

---

## About This Project

This application was developed as a **client deliverable** by a developer with extensive experience building similar platforms across **multiple technologies**. It includes **integrated payment gateways** (Razorpay, Stripe, Cashfree), **advanced affiliate and referral logic**, and **production-ready features** such as webhooks, multi-currency support, and automated email notifications.

The codebase is structured for maintainability, follows Laravel conventions, and is ready for deployment with clear configuration and environment setup.

---

## Key Features

### E‑commerce & Checkout
- **Package catalog** with priority-based tiers and detailed pricing (INR & USD)
- **Shopping cart** with add-to-cart and checkout flow
- **Referral checkout** with sponsor ID validation and autocomplete
- **Coupon system** with unique codes and validation at checkout
- **Order lifecycle**: Pending → Processing → Completed / Failed, with full payment tracking

### Payment Integration
- **Razorpay** — Card, UPI, netbanking (India)
- **Stripe** — Card payments and Checkout Sessions (international / USD)
- **Cashfree** — Alternative Indian payment gateway
- **Stripe webhooks** for reliable payment confirmation and order completion
- Support for **full payment** and **partial/processing** payment flows
- **Sponsor payment** flow for affiliate commission payouts

### Multi-Currency
- **INR (₹)** and **USD ($)** with session-based currency selection
- Currency-aware pricing, orders, and affiliate payouts
- Admin reporting split by currency (INR/USD paid and unpaid totals)

### Affiliate & Referral System
- **Multi-level referral commissions** (0th Level through 5th Level)
- **Referral links** and tracking
- **Direct sales commissions** and **team performance bonus** reporting
- **Request payment** workflow with admin accept/reject
- **Withdrawal history** and **tax invoice** generation
- **Leaderboard** and **network tree** views for affiliates

### Content & Learning
- **Packages** and **courses** with authors and media
- **Lectures** and **content** (rich text with image upload)
- **User package access** based on purchase and priority
- **Certificate download** for completed courses
- **Webinars** and **offers** with dedicated listing and detail pages

### User (Affiliate) Dashboard
- Dashboard with recent sales and referrals
- **Profile** with country/state/city, **KYC upload**, and **bank details**
- **My courses**, **upgrade packages**, and **course grid/list** views
- **My referrals**, **referral link** management, and **coupon codes**
- **Direct sales commissions**, **team performance bonus**, **withdrawal history**
- **Tax invoices**, **request payment**, **support**, **live training**
- **Network** view and **leaderboard**

### Admin Panel
- **Dashboard** with today’s orders and affiliate/referral totals (INR & USD)
- **Package, course, lecture, content** CRUD with status toggle and media
- **User management** (create, edit, status, bank details, password reset, package orders)
- **Order management** with quick view, complete/processing/fail actions, payment sync
- **Affiliates** list and **affiliate detail** with DataTables and filters
- **Referrals** CRUD and bulk status update
- **User package upgrade** and **sponsor/passive income payment** actions
- **Webinars**, **offers**, **authors**, **coupons** with status and validation
- **Notifications** (email and SMS) with rich editor and image upload
- **Settings** and **profile** (including change password)
- **Network** view for hierarchy visualization

### Notifications & Mail
- **Place order** confirmation
- **Payment confirmation**
- **Referral** and **passive income** notifications
- **New user** welcome
- **Reset password** and **admin notification**
- **Payment notification** to affiliates
- **Income change** (admin and congregation)
- Customizable mail templates and layout

### Security & Auth
- **Dual guard**: Admin and User (affiliate) with separate login
- **Forgot password** and **reset password** flow
- **Laravel Sanctum** for API auth
- **CSRF** and validation on forms and checkout

### Developer & Operations
- **Log viewer** (opcodesio/log-viewer) for debugging
- **Spatie Media Library** for consistent file handling
- **Intervention Image** for image processing
- **Yajra DataTables** for admin tables
- **Guzzle** for HTTP (e.g. Cashfree API)
- **Vite** for front-end assets
- **Database migrations** and **seeders** for packages, countries, states, cities, authors

---

## Technology Stack

| Layer        | Technology |
|-------------|------------|
| Backend     | PHP 8.0+, Laravel 9.x |
| Frontend    | Blade, Vite, JavaScript, CSS |
| Database    | MySQL (via Laravel migrations) |
| Payments    | Razorpay, Stripe, Cashfree |
| Cart        | darryldecode/cart |
| Media       | Spatie Laravel Media Library, Intervention Image |
| Data tables | Yajra Laravel DataTables |
| API auth    | Laravel Sanctum |
| Mail        | Laravel Mail (SMTP) |

---

## Requirements

- PHP 8.0.2 or higher
- Composer
- Node.js & npm (for Vite)
- MySQL 5.7+ / MariaDB
- Web server (Apache/Nginx) or `php artisan serve`

---

## Installation

1. **Clone the repository**
   ```bash
   git clone <repository-url> chaldigital
   cd chaldigital
   ```

2. **Install PHP dependencies**
   ```bash
   composer install
   ```

3. **Environment**
   ```bash
   cp .env.example .env
   php artisan key:generate
   ```
   Edit `.env` and set:
   - `APP_NAME`, `APP_URL`
   - `DB_*` (database name, user, password)
   - `MAIL_*` (for transactional emails)
   - `RAZORPAY_KEY`, `RAZORPAY_SECRET`
   - `STRIPE_KEY`, `STRIPE_SECRET`, `STRIPE_WEBHOOK_SECRET`
   - `CASHFREE_APP_KEY`, `CASHFREE_SECRET_KEY`, `CASHFREE_BASE_URL` (if using Cashfree)

4. **Database**
   ```bash
   php artisan migrate
   php artisan db:seed
   ```

5. **Front-end assets**
   ```bash
   npm install
   npm run build
   ```
   For development: `npm run dev`

6. **Storage link**
   ```bash
   php artisan storage:link
   ```

7. **Run the application**
   ```bash
   php artisan serve
   ```
   - Front/checkout: `http://localhost:8000`
   - Admin: `http://localhost:8000/admin`
   - Affiliate login: `http://localhost:8000/affiliate-login`

---

## Payment Webhooks

- **Stripe**: Configure your Stripe webhook to point to  
  `POST /api/stripe/webhook`  
  and set `STRIPE_WEBHOOK_SECRET` in `.env`.  
  The app handles `checkout.session.completed` and `checkout.session.payment_failed`.

- For **Razorpay** and **Cashfree**, configure their webhook URLs in the respective dashboards to your deployed base URL and any routes you expose for them (see `routes/web.php` / `routes/api.php` for existing payment callbacks).

---

## Project Structure (Overview)

```
app/
├── Http/Controllers/
│   ├── Admin/          # Dashboard, packages, courses, users, orders, affiliates, referrals, etc.
│   ├── User/           # Dashboard, payments, profile, referrals, courses, upgrade, invoices
│   ├── Auth/            # Login, forgot password
│   ├── CheckoutController.php
│   ├── CartController.php
│   ├── StripeController.php
│   └── WebhookController.php   # Stripe webhooks
├── Mail/                # Place order, payment confirmation, referral, passive income, etc.
├── Models/              # User, Admin, Order, Package, Course, AffiliatePayment, etc.
├── Http/Middleware/     # SetDefaultCurrency, auth guards
config/                  # app, auth, database, services (Stripe), mail, etc.
database/migrations/     # Full schema (users, packages, orders, payments, affiliates, etc.)
resources/views/         # admin, front-end, user, emails
routes/
├── web.php              # All web routes (guest, admin, user)
└── api.php              # Sanctum + Stripe webhook
```

---

## License

This project is proprietary software developed for the client. All rights reserved.

---

## Developer Note

This platform was built by a developer with prior experience in:

- Delivering **similar affiliate, course, and e‑commerce projects** using various frameworks and technologies
- **Integrating multiple payment gateways** (Razorpay, Stripe, Cashfree, and others) with webhooks and multi-currency handling
- Implementing **advanced features** such as multi-level referral commissions, sponsor validation, request payment workflows, tax invoices, and admin reporting

The architecture is designed to be clear, extensible, and ready for production use with proper environment and payment configuration.
