# Void Inventory Manager

Void Inventory Manager is a full-stack inventory and sales management application for shops and boutique businesses. It helps store owners and staff track products, manage stock, record sales and refunds, monitor revenue/profit, maintain customer records, and configure shop-level settings.

The project is built with Next.js, MongoDB, and NextAuth, and is designed for internal business use with role-based access for developers, shop admins, and editors.


## App summary

This application provides:

- Product catalog management with add, edit, delete, search, and stock updates
- Sales and refund tracking with real-time stock deduction/adjustment
- Daily and date-range sales reporting
- Revenue, profit, and sold-quantity overview cards
- Customer tracking and sales attribution
- Admin and editor access control by shop
- Multi-currency pricing and base-currency conversion
- Printable and downloadable sales reports
- Real-time product/sales updates over Socket.IO

## Main product goals

The app is designed to help a business manage the daily operational flow of selling physical products:

1. Register a shop and admin account
2. Add products with cost price, selling price, quantity, and unit type
3. Sell or refund products
4. Track profitability and daily performance
5. Manage multiple users within the same shop
6. Export or print sales records for reporting

## Features

### Product management

- Add new products with title, description, cost price, selling price, stock quantity, and unit type
- Search products by title
- Sort products by fields such as name, price, or quantity
- Restock inventory
- Edit product metadata and pricing
- Delete products
- Enforce stock validation before a sale is processed

### Sales and refunds

- Record sales with quantity, payment method, and customer information
- Support refund operations that reverse stock and revenue/profit
- Track sales by day and date range
- Summarize total revenue, profit, sold quantity, and refund totals
- Store transaction details per product line

### Reporting and dashboard

- Dashboard cards for today’s revenue, profit, sold quantity, and refunds
- Sales analytics grouped by date
- Search sales by product name
- Filter by date or date range
- Display cash and MoMo payment totals
- Printable and downloadable report exports

### Customer and account management

- Add and view customers
- Associate customer purchases with their records
- Admin can manage editor accounts for their shop
- Shop-specific data isolation per business

### Currency management

- Configure a base currency for each shop
- Store exchange-rate data for conversion between currencies
- Use currency-specific product cost and selling prices
- Convert item values into shop base currency for reporting

### Roles and permissions

The app supports multiple authorization layers:

- Superdeveloper: platform-level developer account
- Developer: internal app developer account
- Admin: shop owner or manager
- Editor: staff member assigned to a shop

The authentication flow is handled by NextAuth with credential-based login.

## Tech stack

- Frontend: Next.js 16, React 19
- Styling: Tailwind CSS
- Backend: Next.js API routes
- Database: MongoDB
- Authentication: NextAuth.js with credentials
- Real-time events: Socket.IO
- PDF / print generation: jsPDF, html2canvas, react-to-print
- Charts: Recharts
- Password hashing: bcryptjs
- HTTP client: axios

## Project structure

```text
.
├── components/
│ <code>
├── lib/
│   ├── auth.js
│   ├── mongodb.js
│   └── socket.js
├── pages/
│   <source code>
├── public/
├── styles/
│   └── globals.css
├── .gitignore
├── eslint.config.mjs
├── next.config.ts
├── package.json
├── package-lock.json
├── tsconfig.json
└── README.md
```

## Database design

The app uses MongoDB and separates shop data into per-shop databases. The code expects a `master_Db` database for global auth and shop metadata, and per-shop databases named in the pattern:

```text
shop_<ShopName>
```

Examples of collections used across the app include:

- `shops`
- `admins`
- `editors`
- `developers`
- `superdevelopers`
- `products`
- `sales`
- `customers`
- `currencies`

The app keeps shop data isolated by business name, which makes it possible to support multiple shops with their own product and sales records.

## Authentication and authorization flow

User login is handled through a custom NextAuth credentials provider configured in `pages/api/auth/[...nextauth].js`.

It can authenticate:

- superdevelopers
- developers
- shop admins
- shop editors

The session stores:

- user ID
- username or developer name
- email
- shop name
- role

A protected route is then available to the logged-in user based on session data and role checks.

## Setup and installation

### Prerequisites

- Node.js 18+ recommended
- MongoDB instance or MongoDB Atlas connection
- npm or yarn

### 1. Clone and install

```bash
npm install
```

### 2. Configure environment variables

Create a `.env.local` file in the project root with values similar to:

```env
MONGODB_URI=mongodb://localhost:27017/your-database
NEXTAUTH_SECRET=your-very-long-secret-key
NEXTAUTH_URL=http://localhost:3000
```

Notes:

- `MONGODB_URI` is required because the app opens a MongoDB client in `lib/mongodb.js`.
- `NEXTAUTH_SECRET` is required for JWT/session security.
- `NEXTAUTH_URL` is recommended for local development and production deployments.

### 3. Start the app

Development mode:

```bash
npm run dev
```

Production build:

```bash
npm run build
npm run start
```

### 4. Linting

```bash
npm run lint
```

## Running the app

Once started, the app is usually available at:

```text
http://localhost:3000
```

The landing page presents the app entry point and routes users to login or registration flows.

## Typical user journey

### Shop registration and admin setup

- A shop admin registers through the app
- Shop metadata is stored in the master database
- A shop-specific database is created or used for the shop’s operations
- The admin account is assigned the `admin` role

### Product workflow

- Admin/editor adds product records
- Product stock levels are managed via restock or sales actions
- All sales update stock immediately

### Sales workflow

- A sale is recorded against a product and quantity
- Revenue and profit are calculated based on product cost and selling price
- Payment method and customer data may be attached
- Refunds are recorded as negative stock/value adjustments

### Reporting workflow

- The dashboard summarizes the current day
- Sales pages allow filtering and searching by product and date
- Reports can be printed or exported for business review

## API overview

The project exposes Next.js API routes under `pages/api/` for the following major actions:

- `/api/product` — create, read, edit, restock, and delete products
- `/api/sales` — record sales, process refunds, fetch sales data, and group totals
- `/api/customer` — customer management and customer sales history
- `/api/currency` — currency configuration and exchange-rate access
- `/api/register` — user registration
- `/api/login` — login flow support
- `/api/editorRegister` — add or remove editor accounts
- `/api/getAdminInfo` — fetch current admin details
- `/api/settings` — shop settings support
- `/api/socket` — real-time socket access

## App behavior and assumptions

- The app expects product and sales data to be organized around a shop context.
- Each shop works with its own isolated MongoDB database.
- Currency values are stored and converted using configured exchange rates.
- Sales totals are computed in the shop base currency.
- Stock quantities are validated before sale processing.

## Security notes

This application includes basic production-ready patterns such as:

- Password hashing via `bcryptjs`
- NextAuth credential authentication
- Per-shop data access based on the authenticated session
- Session-based user identity and access control

For production use, it is recommended to:

- use strong secrets in environment variables
- set secure cookie and session settings
- keep MongoDB credentials and secrets out of source control
- implement additional role-check guards for highly sensitive admin operations

## Development notes

- The app leverages both client-side and server-side logic in the same project structure.
- Some pages and components are named according to shop operations and internal tooling rather than a strict enterprise-style architecture.
- Real-time synchronization is included through Socket.IO to refresh product and sales data in connected clients.

## Future improvement opportunities

The project could be extended with:

- stricter role-permission enforcement on all routes
- admin audit trail / activity logs
- CSV export for products and sales
- invoice generation and printable receipts
- stock alerts and reorder recommendations
- dashboard filters by employee or payment method
- multi-tenant improvements and deployment automation

## License

No explicit license file is included in this repository at the moment. If you plan to distribute or deploy this app publicly, add a license file and define the ownership and usage terms clearly.

## Conclusion

Void Inventory Manager is a practical inventory and sales tracking system designed for small businesses that need to manage stock, sales, customer relationships, and operating reports from one web dashboard. It is a self-contained monolithic Next.js application that brings together authentication, inventory logic, reporting, and shop-level data management in a single codebase.
